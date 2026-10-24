[Indice della sezione](indice.md) · [Indice](../indice.md)

# Rete e DNS

Gli esempi usano nomi e indirizzi generici, non la configurazione reale.

| Traffico | Percorso |
|---|---|
| Tool LAN | DNS locale -> reverse proxy -> VM applicativa |
| Accesso remoto privato | Tailscale subnet router -> LAN |
| Web selezionato | Cloudflare Access/Tunnel -> origin |
| Share SMB | LAN o Tailscale, nessuna pubblicazione Internet |

Il router mantiene gli indirizzi stabili e distribuisce il DNS locale ai client.
Per risolvere i nomi interni via VPN è configurato uno split DNS verso il resolver
raggiungibile sulla subnet autorizzata. La raggiungibilità del nodo overlay
non implica che ogni sua porta sia autorizzata: resolver e policy devono coincidere.

Nessun port forwarding sul router. Il DNS e il proxy sono concentrati sul nodo
di rete; la sua disponibilità è una dipendenza nota. Il proxy offre HTTPS
con CA locale sui dispositivi personali e redirect da HTTP. La chiave della CA
resta separata dal server; ogni client deve importare il certificato pubblico.
La cifratura e la verifica TLS dei singoli upstream restano aspetti distinti.

Le UI amministrative ordinarie, il monitoraggio e la homepage hanno nomi DNS
privati sotto Nginx, inclusa una route predisposta per il laboratorio spento.
Questa risponde temporaneamente 503 senza avviare la VM.
Il proxy verifica il certificato dell’hypervisor usando la sua CA pubblica;
WebSocket e protocolli applicativi restano da verificare per ciascun servizio.

Per le applicazioni pubblicate Cloudflare gestisce il certificato lato browser;
questo non sostituisce i certificati dell’accesso LAN diretto. Non sono stati
aggiunti client ACME esterni. Il password manager non ha una route pubblica.
Il tunnel ha due connettori su host distinti: un origin loopback non è
intercambiabile fra le repliche; gli origin condivisi devono essere raggiungibili
da entrambe.

Una pagina statica di indisponibilità gestisce nomi privati non configurati e
backend fermi. DNS e wildcard TLS della zona locale permettono di raggiungere
il proxy anche con un nome locale sconosciuto; HTTP 404 e 503 restano errori,
mentre i codici applicativi di autenticazione e le risorse mancanti sono
preservati. Un problema DNS esterno o un proxy irraggiungibile resta gestito
dal browser, perché la richiesta non arriva alla pagina.
