# Modello di sicurezza

## Principi

1. Nessun port forwarding inbound sul router.
2. Amministrazione privata su overlay autenticata e cifrata.
3. Accesso identity-aware davanti alle applicazioni pubblicate.
4. Identità CI temporanee e con minimo privilegio.
5. Utenti di deploy ristretti e operazioni privilegiate in allowlist.
6. Nessun plaintext secret o decrypt identity privata in source control o
   nelle immagini container.
7. Artefatti immutabili identificati dal Git SHA.
8. Backup indipendenti, verificati e sottoposti a prove di ripristino.

## Confini di fiducia

Proxmox è il livello di virtualizzazione ad alta fiducia. La VM Docker costituisce una piattaforma applicativa separata. I progetti Compose isolano le risorse ordinarie ma condividono il kernel, quindi ospitano solo workload compatibili con il livello di fiducia accettato. Il nodo di rete è infrastruttura, non un host applicativo generico. Il sistema di backup forma un confine amministrativo e storage distinto.

Il futuro control plane userà API ristrette e operazioni predefinite. Non esporrà una shell arbitraria e non monterà il socket Docker in un'applicazione web.

## Gestione dei segreti

Gli esempi usano placeholder come `admin`, `deploy-user`, `example.com` e `192.168.x.x`. Credenziali e sessioni reali vivono in secret store o file runtime protetti. Prima della condivisione, log e bundle vengono controllati per header di autorizzazione, cookie, URL firmati e valori d'ambiente.

Per i secret statici e distribuibili dal desired state è stata selezionata
un'architettura SOPS + age. I file cifrati potranno vivere nel repository
privato insieme ai recipient pubblici; ogni workstation autorizzata avrà invece
una identity privata distinta, conservata fuori da Git. Una capacità di
recovery offline separata dalle workstation operative evita che una singola
macchina sia l'unico percorso di ripristino.

La rimozione di un recipient non revoca retroattivamente vecchi commit, backup
o copie del ciphertext. Aggiornare i recipient e ricifrare un file è distinto
dal ruotare la password o credenziale realmente usata dal servizio. Il
ciphertext resta quindi security-relevant anche quando è versionabile.

Questa scelta riguarda secret statici/deployable e non pretende di sostituire
un eventuale futuro secret server quando emergessero requisiti concreti di
credenziali dinamiche, workload identity, audit centralizzato o valori
short-lived. Tooling e consumo da Ansible sono fasi future.

La sicurezza è verificata in CI: il repository privato rifiuta chiavi e artefatti pericolosi, mentre l'albero pubblico generato viene analizzato separatamente per identificatori infrastrutturali e contenuti simili a credenziali.

## File sharing

Il servizio SMB richiede autenticazione e non consente guest access. Un gruppo
Unix dedicato governa i permessi della share; il bit setgid sulle directory
condivise mantiene coerente il gruppo dei nuovi file e delle sottodirectory.
Credenziali e identità reali non fanno parte della configurazione pubblicata.

Il servizio resta raggiungibile soltanto dalla LAN e dalla rete Tailscale. La
dipendenza systemd dal mount del disco dati evita un fallback silenzioso sul
filesystem di sistema quando lo storage dedicato non è disponibile.
