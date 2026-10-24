[Indice della sezione](indice.md) · [Indice](../indice.md)

# Piattaforma Docker

Docker gira in una VM Debian su Proxmox. L'hypervisor resta dedicato alle VM;
i tool condividono il runtime, mantenendo progetti Compose e dati separati.
Ogni stack ha rete, ambiente privato, dati persistenti e gestione del ciclo di vita.

Open WebUI conserva utenti/chat fuori dal container e usa Ollama su una
workstation GPU in LAN. La UI sopravvive ai redeploy grazie al mount dati;
la disponibilità dell'inferenza dipende dalla workstation.
Portainer è il pannello amministrativo, non una sostituzione delle configurazioni.
Gitea rootless usa PostgreSQL separato; login e operazioni Git HTTPS verificati
anche tramite tunnel pubblico. Registrazione aperta e runner CI sono disabilitati.
Il password manager ufficiale Lite gira sul nodo di rete, con dati persistenti
privati su USB, controllo del mount e backup sul disco dell’hypervisor.
Il suo accesso remoto passa esclusivamente dalla VPN.

Il runtime container ha storage separato dai dati applicativi. Health check
presenti dove configurati; limiti risorse e logging uniforme restano da
standardizzare. I tag immagini correnti non sono tutti immutabili.

Il nodo di rete esegue solo i container infrastrutturali necessari, mentre
DNS, VPN, proxy e Samba sono servizi nativi. Tool accessori dismessi non sono
ripristinati automaticamente.

## Laboratorio

La VM distinta per Kubernetes/Jenkins e deploy di test è stata creata.
K3s e Jenkins sono installati tramite Compose con dati persistenti e avvio
manuale. Nessun riavvio automatico dei container; il laboratorio si spegne a
fine uso. La configurazione di pipeline resta un esercizio successivo.
