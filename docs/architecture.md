# Architettura

## Obiettivi progettuali

L'homelab privilegia failure domain comprensibili, servizi riproducibili, bassi consumi a riposo e capacità di ripristino, evitando complessità di orchestrazione non necessaria.

```text
Internet
   |
edge identity-aware
   |
network-node ---- rete overlay privata
   |
LAN
   +-- hypervisor
   |     `-- VM docker-host
   |           `-- progetti Compose isolati
   +-- network-node
   |     `-- storage USB dedicato
   |           `-- file share SMB
   `-- backup-server
```

L'hypervisor gestisce ciclo di vita e storage delle VM, senza ospitare direttamente container applicativi. Docker gira in una VM Debian per offrire un ambiente convenzionale, evitare container privilegiati annidati e creare un confine netto per backup e restore.

Una sola VM Docker riduce consumo di risorse e manutenzione. Ogni stack mantiene progetto Compose, rete, dati persistenti, configurazione, health check e pipeline indipendenti. Un workload passa a una VM dedicata quando richiede un confine di fiducia, un kernel, una disponibilità o risorse differenti.

## AI locale

```text
Browser
   `-- Open WebUI (container sul Docker host)
         `-- LAN --> Ollama (workstation GPU)
                       `-- modello locale / NVIDIA GPU
```

Open WebUI fornisce interfaccia e orchestrazione in un container, con database,
cronologia e configurazione persistiti fuori dal lifecycle del container. Ollama
gira su una workstation separata dotata di GPU: il traffico di inferenza resta
sulla LAN e il modello testato ha usato accelerazione NVIDIA. Questa separazione
mantiene il workload applicativo sul Docker host e assegna il calcolo di
inferenza al nodo più adatto, senza descriverlo come server always-on.

Il nodo di rete sempre acceso fornisce DNS, routing privato, connettività tunnel e monitoraggio. Il server di backup è fisicamente indipendente e può rimanere spento fuori dalle finestre operative.

## Ingress applicativo

La baseline del central ingress HTTP è implementata sul nodo di rete,
separandola dai container applicativi. Un primo backend applicativo è stato
verificato end-to-end attraverso il reverse proxy:

```text
edge identity-aware
   `-- tunnel connector
         `-- reverse proxy sul network-node
               `-- backend sul docker-host
```

Le route restanti e i servizi saranno migrati uno alla volta senza cambiare
subito le porte backend. Sulla LAN il pattern è
`service.home.arpa -> central ingress`: i record dei servizi proxati puntano al
nodo di ingress, non direttamente ai backend. Convergenza del tunnel, TLS
interno e restrizione dell'accesso diretto agli upstream restano fasi separate.
Il guasto del nodo di rete rende indisponibile l'ingress, mentre i backend
possono continuare a funzionare.

## File share

Il nodo di rete ospita anche un file server Samba. I dati risiedono su un disco
USB 3.0 dedicato, separato dal filesystem di sistema, collegato tramite UAS e
formattato con il filesystem Linux nativo `ext4`. Un mount persistente rende
disponibile una struttura concettuale come questa:

```text
/srv/storage/
├── share/
│   ├── Documents/
│   ├── Shared/
│   └── Transfer/
└── music/
```

`music/` è soltanto predisposta per un futuro music server. Samba espone la
directory condivisa tramite SMB; l'unità systemd del servizio dipende dal mount
dello storage, così un disco non disponibile non causa l'esposizione
accidentale di una directory vuota sul filesystem di sistema.

Sono stati verificati filesystem, autenticazione SMB, lettura, scrittura,
trasferimento file e accesso da macOS da remoto tramite Tailscale. Restano da
verificare i client Linux e Windows e il comportamento definitivo dello standby
del disco dopo l'uso SMB. Non sono stati eseguiti benchmark di throughput.

## Failure domain

- Il guasto di uno stack non deve coinvolgere progetti Compose estranei.
- La VM Docker si recupera tramite backup VM e configurazione versionata.
- Il nodo di rete richiede backup dedicato perché concentra servizi infrastrutturali.
- Il file share è storage operativo; una futura copia di backup avrà un ciclo di vita indipendente.
- La perdita dell'hypervisor di produzione non deve distruggere anche il backup primario.

Il disaster recovery procede dall'infrastruttura verso l'esterno: rete, hypervisor, piattaforma container, applicazioni e infine strumenti accessori.
