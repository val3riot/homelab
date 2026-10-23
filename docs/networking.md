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
di rete; la sua disponibilità è una dipendenza nota. TLS interno e restrizioni
ulteriori degli upstream sono attività separate, non già completate.
