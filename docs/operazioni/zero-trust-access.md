[Indice della sezione](indice.md) · [Indice](../indice.md)

# Accesso

Tailscale collega dispositivi autorizzati alla LAN tramite subnet router.
Le policy stabiliscono reti e porte raggiungibili; il DNS privato richiede
anche che il resolver configurato sia raggiungibile con quelle policy.

Cloudflare Tunnel apre connessioni in uscita verso l'edge. Le UI amministrative
selezionate usano Cloudflare Access oltre al login applicativo. Il servizio Git
usa autenticazione applicativa compatibile con i client Git HTTPS; le policy
vanno definite per servizio, senza presumere che una login interattiva sia
compatibile con tutti i client. Password manager e share SMB restano LAN/VPN.

L'amministrazione SSH usa chiavi dedicate e host-key checking. L'identità
ristretta della precedente pipeline applicativa è disabilitata dopo la
dismissione del servizio. Il modello di accesso CI sarà rivalutato nel
laboratorio futuro; non si descrive il vecchio deploy come ancora attivo.
