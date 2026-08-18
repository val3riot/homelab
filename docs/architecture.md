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
   `-- backup-server
```

L'hypervisor gestisce ciclo di vita e storage delle VM, senza ospitare direttamente container applicativi. Docker gira in una VM Debian per offrire un ambiente convenzionale, evitare container privilegiati annidati e creare un confine netto per backup e restore.

Una sola VM Docker riduce consumo di risorse e manutenzione. Ogni stack mantiene progetto Compose, rete, dati persistenti, configurazione, health check e pipeline indipendenti. Un workload passa a una VM dedicata quando richiede un confine di fiducia, un kernel, una disponibilità o risorse differenti.

Il nodo di rete sempre acceso fornisce DNS, routing privato, connettività tunnel e monitoraggio. Il server di backup è fisicamente indipendente e può rimanere spento fuori dalle finestre operative.

## Failure domain

- Il guasto di uno stack non deve coinvolgere progetti Compose estranei.
- La VM Docker si recupera tramite backup VM e configurazione versionata.
- Il nodo di rete richiede backup dedicato perché concentra servizi infrastrutturali.
- La perdita dell'hypervisor di produzione non deve distruggere anche il backup primario.

Il disaster recovery procede dall'infrastruttura verso l'esterno: rete, hypervisor, piattaforma container, applicazioni e infine strumenti accessori.
