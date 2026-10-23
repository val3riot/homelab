<!-- Generated from private allowlisted sources. Do not edit the exported repository directly. -->
# Homelab

Homelab basato su Proxmox, una VM Debian per tool Docker e un nodo a basso
consumo per DNS, ingress, VPN, storage condiviso e monitoraggio.
Questa documentazione descrive l'architettura senza indirizzi o credenziali reali.

## Attivo

- Docker Compose con dati persistenti separati dai container.
- Open WebUI sulla VM e Ollama su una workstation GPU in LAN.
- Portainer, DNS locale, Uptime Kuma e Samba.
- Tailscale per accesso privato; Cloudflare Tunnel/Access per servizi selezionati.
- Nginx per le route LAN; Ansible per configurazione e verifica del drift.
- GitHub Actions per export controllato della documentazione pubblica.

## Pianificato

VM laboratorio separata per Kubernetes/Jenkins, backup centralizzato PBS e
gestione secret SOPS/age. Questi componenti non sono ancora implementati.
La precedente pipeline applicativa è dismessa; il suo modello resta documentato
come esperienza, non come servizio corrente.

## Guide

- [Architettura](docs/architecture.md)
- [Rete e DNS](docs/networking.md)
- [Piattaforma Docker](docs/docker-platform.md)
- [Accesso](docs/zero-trust-access.md)
- [CI/CD](docs/cicd.md)
- [Backup pianificato](docs/backup-strategy.md)
- [Monitoraggio](docs/monitoring.md)
- [Sicurezza](docs/security-model.md)
- [Inventario di esempio](examples/hosts.yaml)

Il pubblico è generato: export allowlisted -> validazione -> branch automatico
-> PR -> merge manuale. Nessuna sincronizzazione automatica verso il privato.
