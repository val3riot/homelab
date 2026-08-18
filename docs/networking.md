# Networking

La rete interna usa indirizzi privati come `192.168.x.x`; gli esempi non descrivono subnet, route, dispositivi o policy reali.

| Traffico | Percorso | Esposizione |
|---|---|---|
| Client locale | LAN verso servizio | Privata |
| Amministrazione remota | Tailscale verso servizio | Overlay privata autenticata |
| File sharing | SMB da LAN o Tailscale | Privata |
| Web selezionato | Cloudflare Tunnel verso origin | Nessuna regola inbound sul router |
| Backup | Nodi di produzione verso backup server | Privata e pianificata |

Le interfacce amministrative non sono esposte direttamente a Internet. Tailscale fornisce identità e trasporto cifrato per operatori e identità CI temporanee. Cloudflare Tunnel stabilisce connessioni in uscita per i servizi HTTP selezionati; Cloudflare Access verifica l'identità prima che la richiesta raggiunga l'origin.

Il design evita il port forwarding. Firewall e policy overlay applicano il minimo privilegio: un'identità di automazione raggiunge soltanto l'endpoint di deploy necessario, mentre SMB resta limitato a LAN e overlay privata. Il file share non è pubblicato su Internet né tramite Cloudflare Tunnel; un esempio di endpoint client è `smb://server/Homelab`.

DNS e monitoraggio risiedono sul nodo di rete a basso consumo. Questa concentrazione semplifica le operazioni ma costituisce una dipendenza nota, mitigata da backup della configurazione, ordine di ripristino documentato e futura ridondanza.
