# Accesso

Tailscale collega dispositivi autorizzati alla LAN tramite subnet router.
Le policy stabiliscono reti e porte raggiungibili; il DNS privato richiede
anche che il resolver configurato sia raggiungibile con quelle policy.

Cloudflare Tunnel apre connessioni in uscita verso l'edge. Le applicazioni
web selezionate sono protette da Cloudflare Access, oltre al proprio login.
La share SMB resta limitata a LAN/VPN.

L'amministrazione SSH usa chiavi dedicate e host-key checking. L'identità
ristretta della precedente pipeline applicativa è disabilitata dopo la
dismissione del servizio. Il modello di accesso CI sarà rivalutato nel
laboratorio futuro; non si descrive il vecchio deploy come ancora attivo.
