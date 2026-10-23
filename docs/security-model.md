# Sicurezza

- Nessun port forwarding sul router.
- Accesso remoto privato tramite overlay autenticata e subnet policy.
- Cloudflare Access e login applicativo per i servizi web selezionati.
- SMB con autenticazione, gruppo dedicato e nessun guest access.
- Secret, chiavi private e dati applicativi fuori dal source control.
- Export pubblico allowlisted e validato separatamente dal repository privato.

Proxmox è il confine di virtualizzazione. I tool Compose condividono il kernel
della VM; esperimenti con diverso livello di fiducia avranno una VM dedicata.
Il socket Docker è un accesso privilegiato del pannello amministrativo, non
un endpoint da assegnare indiscriminatamente a nuove applicazioni.

SOPS + age è una scelta progettuale: tooling e integrazione non sono operativi.
In futuro saranno versionabili ciphertext e recipient pubblici, mantenendo
identity private e recovery offline fuori da Git. Ricifrare non equivale a
revocare una credenziale presente in vecchi commit o copie.

Backup indipendenti e restore provati sono obiettivi ancora da implementare.
