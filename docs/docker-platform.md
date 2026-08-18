# Piattaforma Docker

## Perché Docker gira in una VM Proxmox

Una VM Debian convenzionale separa l'amministrazione dell'hypervisor dalle operazioni applicative. Evita di concedere agli strumenti applicativi accesso al nodo Proxmox, segue i normali percorsi di aggiornamento Docker e rende l'intero runtime una singola unità coerente di backup e restore.

## Perché una VM condivisa

Un `docker-host` condiviso è proporzionato per workload personali fidati e contenuti: riduce memoria a riposo, patching e configurazioni di base duplicate. Non viene però gestito come un unico stack indistinto.

Ogni applicazione possiede:

```text
progetto Compose
├── file Compose
├── ambiente fornito fuori da Git
├── reti dedicate
├── dati persistenti dedicati
├── health check
└── versioni immutabili delle immagini
```

I nomi progetto evitano collisioni e, per impostazione predefinita, le applicazioni non condividono volumi scrivibili. Limiti di risorse e rotazione log contengono i noisy neighbor. Un workload ottiene una VM dedicata quando cambiano livello di fiducia, requisiti kernel, disponibilità o profilo risorse.

La piattaforma privilegia file Compose dichiarativi e host ricostruibili. Un'interfaccia grafica può aiutare l'osservazione, ma la configurazione versionata resta autorevole.
