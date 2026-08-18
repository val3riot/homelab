# Accesso Zero Trust

Le decisioni di accesso dipendono dall'identità e da policy esplicite, non dalla sola posizione di rete.

## Amministrazione privata

Tailscale collega i dispositivi approvati ai servizi privati senza aprire porte inbound sul router. Il subnet routing è limitato alle reti necessarie. L'automazione usa un'identità temporanea con tag e ACL ristrette, senza ereditare l'accesso esteso dell'operatore.

## Pubblicazione web

Cloudflare Tunnel stabilisce connessioni in uscita da `network-node` verso l'edge. Hostname come `app.example.com` possono essere protetti da Cloudflare Access prima dell'inoltro all'origin. I servizi amministrativi restano privati o richiedono policy di identità più forti.

## Identità di deploy

L'account `deploy-user` è dedicato: login interattivo disabilitato, credenziali vincolate e ingresso SSH limitato a operazioni in allowlist. Non appartiene al gruppo Docker e non dispone di una shell generica. Le azioni privilegiate passano attraverso wrapper ristretti.

Questo limita l'impatto di un job CI compromesso preservando il deploy automatico.
