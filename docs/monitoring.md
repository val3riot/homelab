# Monitoraggio

Il monitoraggio parte dai segnali direttamente utili alle operazioni:

- health check HTTP interni ed esterni;
- stato di container e progetti Compose;
- pressione su CPU, memoria, filesystem e storage Docker;
- disponibilità di hypervisor e VM;
- completamento e verifica backup, capacità del datastore;
- stato del mount e salute SMART del disco dedicato al file share;
- salute di tunnel, DNS e routing privato.

Un servizio di uptime su `network-node` offre controlli semplici e notifiche. Stato e log nativi delle piattaforme restano la fonte per la diagnosi. Le dashboard riassumono, ma non sostituiscono alert azionabili e runbook documentati.

Per lo storage USB del file share sono stati completati il controllo SMART, uno
short SMART self-test e la verifica del filesystem. `smartd` è configurato per
non svegliare inutilmente un disco già in standby. Il comportamento definitivo
dello standby dopo attività SMB resta oggetto di verifica.

La fase successiva introduce metriche centralizzate con soglie, responsabilità e controllo del rumore espliciti. Gli endpoint amministrativi restano in LAN o sulla rete overlay. Un eventuale stato pubblico espone solo disponibilità aggregata, senza indirizzi interni o identità dei componenti.
