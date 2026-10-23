# Piattaforma Docker

Docker gira in una VM Debian su Proxmox. L'hypervisor resta dedicato alle VM;
i tool condividono il runtime, mantenendo progetti Compose e dati separati.
Ogni stack ha rete, ambiente privato, dati persistenti e gestione del ciclo di vita.

Open WebUI conserva utenti/chat fuori dal container e usa Ollama su una
workstation GPU in LAN. La UI sopravvive ai redeploy grazie al mount dati;
la disponibilità dell'inferenza dipende dalla workstation.
Portainer è il pannello amministrativo, non una sostituzione delle configurazioni.

Il runtime container ha storage separato dai dati applicativi. Health check
presenti dove configurati; limiti risorse e logging uniforme restano da
standardizzare. I tag immagini correnti non sono tutti immutabili.

Il nodo di rete esegue solo i container infrastrutturali necessari, mentre
DNS, VPN, proxy e Samba sono servizi nativi. Tool accessori dismessi non sono
ripristinati automaticamente.

## Laboratorio pianificato

Una VM distinta ospiterà Kubernetes, Jenkins e deploy di test. Non è ancora
creata; risorse e topologia vanno definite prima dell'installazione.
