<!--
This repository is generated from a private infrastructure repository.
Do not edit generated files directly.
-->

# Homelab

Una piattaforma self-hosted orientata alla sicurezza, creata per sperimentare
operazioni infrastrutturali affidabili su piccola scala. Non è soltanto una
raccolta di container: workspace, desired state, provisioning, verifica runtime
e confini dei secret sono progettati come parti di un sistema Git-driven.
Questo repository presenta architettura, automazione e decisioni progettuali
senza esporre l'ambiente reale.

## Architettura

La piattaforma usa Proxmox come confine di virtualizzazione ed esegue i workload containerizzati condivisi in una VM Debian dedicata. I servizi infrastrutturali risiedono su un nodo di rete a basso consumo, mentre un server di backup indipendente offre un confine di ripristino separato.

Docker gira in una VM per mantenere un host Docker convenzionale, lasciare all'hypervisor il solo compito di virtualizzazione e rendere semplice il backup e ripristino della piattaforma applicativa. La VM Docker condivisa è un compromesso intenzionale di efficienza: ogni applicazione resta isolata tramite progetto Compose, reti, volumi, ambiente, health check e pipeline propri.

## Gestione Git-driven

Un bootstrap deterministico prepara il workspace locale senza configurare
credenziali o accesso SSH. Le fonti versionate descrivono host e configurazione;
gli artefatti locali, inclusa l'inventory Ansible generata, sono disposable e
restano separati dal source of truth.

```text
versioned desired state
        → reproducible local tooling
        → generated inventory
        → Ansible check and drift review
        → explicitly authorized apply
        → post-apply convergence check
        → separate runtime evidence
```

Questo processo è stato verificato adottando un primo servizio infrastrutturale
esistente sotto gestione Ansible e ottenendo zero drift gestito nel controllo
post-apply. È una baseline di adozione prudente, non una garanzia universale per
ogni servizio futuro.

## Stack

- Proxmox VE e Proxmox Backup Server
- Debian, Docker Engine e Docker Compose
- Tailscale per l'amministrazione privata
- Cloudflare Tunnel e Access per applicazioni web selezionate
- GitHub Actions e registry OCI per CI/CD
- Ansible per provisioning dichiarativo e verifica del drift
- filtro DNS, uptime check e dashboard dei servizi
- file sharing self-hosted tramite SMB, accessibile da LAN e VPN

## Sicurezza

Il design evita il port forwarding sul router. Il traffico amministrativo usa
una rete overlay privata; le applicazioni pubblicate attraversano un edge
identity-aware. La CI riceve un'identità effimera e raggiunge un utente di
deploy ristretto, le cui operazioni consentite sono esplicite.

Per i futuri secret statici distribuibili è stata scelta l'architettura SOPS +
age: il repository privato potrà contenere ciphertext e recipient pubblici,
mentre ogni workstation autorizzata manterrà la propria identity privata fuori
da Git. Tooling e integrazione Ansible non sono ancora implementati.

## CI/CD

Le pipeline applicative testano e costruiscono immagini container immutabili identificate dallo SHA Git completo. I workflow di deploy scelgono una versione esatta, si connettono attraverso la rete privata, invocano operazioni in allowlist e verificano lo stato di salute. Il modello garantisce tracciabilità e rollback controllato.

## Backup e ripristino

Un Proxmox Backup Server indipendente protegge i dati della VM e del nodo infrastrutturale con retention, verifica e test di restore. File condivisi e dati applicativi sono distinti dai backup: la sincronizzazione non è una strategia di ripristino.

## Monitoraggio

Health check, uptime service, stato dei container, capacità storage e risultati dei job di backup costituiscono il primo livello di monitoraggio. La roadmap aggiunge metriche centralizzate e alert mantenendo private le interfacce amministrative.

## Documentazione

- [Architettura](docs/architecture.md)
- [Networking](docs/networking.md)
- [Piattaforma Docker](docs/docker-platform.md)
- [Accesso Zero Trust](docs/zero-trust-access.md)
- [CI/CD](docs/cicd.md)
- [Strategia di backup](docs/backup-strategy.md)
- [Monitoraggio](docs/monitoring.md)
- [Modello di sicurezza](docs/security-model.md)
- [Esempi generici di inventario](examples/hosts.yaml)

## Evoluzione futura

Le attività pianificate comprendono tooling e onboarding per i secret cifrati,
la loro successiva integrazione nel provisioning, il ripristino bare-metal
testato, maggiori garanzie di backup off-site, metriche e alert, manifest
applicativi standardizzati e un control plane operativo ristretto basato su
azioni consentite e sottoposte ad audit.

## Flusso dei contributi

Questo repository è generato. Le modifiche dirette vengono sovrascritte dall'export successivo. La sincronizzazione `PUBLIC → PRIVATE` non è supportata; i miglioramenti proposti vengono valutati e applicati manualmente alla source of truth privata.
