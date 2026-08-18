<!--
This repository is generated from a private infrastructure repository.
Do not edit generated files directly.
-->

# Homelab

Una piattaforma self-hosted orientata alla sicurezza, creata per sperimentare operazioni infrastrutturali affidabili su piccola scala. Questo repository presenta architettura, automazione e decisioni progettuali senza esporre l'ambiente reale.

## Architettura

La piattaforma usa Proxmox come confine di virtualizzazione ed esegue i workload containerizzati condivisi in una VM Debian dedicata. I servizi infrastrutturali risiedono su un nodo di rete a basso consumo, mentre un server di backup indipendente offre un confine di ripristino separato.

Docker gira in una VM per mantenere un host Docker convenzionale, lasciare all'hypervisor il solo compito di virtualizzazione e rendere semplice il backup e ripristino della piattaforma applicativa. La VM Docker condivisa è un compromesso intenzionale di efficienza: ogni applicazione resta isolata tramite progetto Compose, reti, volumi, ambiente, health check e pipeline propri.

## Stack

- Proxmox VE e Proxmox Backup Server
- Debian, Docker Engine e Docker Compose
- Tailscale per l'amministrazione privata
- Cloudflare Tunnel e Access per applicazioni web selezionate
- GitHub Actions e registry OCI per CI/CD
- filtro DNS, uptime check e dashboard dei servizi

## Sicurezza

Il design evita il port forwarding sul router. Il traffico amministrativo usa una rete overlay privata; le applicazioni pubblicate attraversano un edge identity-aware. La CI riceve un'identità effimera e raggiunge un utente di deploy ristretto, le cui operazioni consentite sono esplicite. I segreti restano fuori da Git.

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

Le attività pianificate comprendono il ripristino bare-metal testato, maggiori garanzie di backup off-site, metriche e alert, manifest applicativi standardizzati e un control plane operativo ristretto basato su azioni consentite e sottoposte ad audit.

## Flusso dei contributi

Questo repository è generato. Le modifiche dirette vengono sovrascritte dall'export successivo. La sincronizzazione `PUBLIC → PRIVATE` non è supportata; i miglioramenti proposti vengono valutati e applicati manualmente alla source of truth privata.
