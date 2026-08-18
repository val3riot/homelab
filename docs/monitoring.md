# Monitoraggio

Il monitoraggio parte dai segnali direttamente utili alle operazioni:

- health check HTTP interni ed esterni;
- stato di container e progetti Compose;
- pressione su CPU, memoria, filesystem e storage Docker;
- disponibilità di hypervisor e VM;
- completamento e verifica backup, capacità del datastore;
- salute di tunnel, DNS e routing privato.

Un servizio di uptime su `network-node` offre controlli semplici e notifiche. Stato e log nativi delle piattaforme restano la fonte per la diagnosi. Le dashboard riassumono, ma non sostituiscono alert azionabili e runbook documentati.

La fase successiva introduce metriche centralizzate con soglie, responsabilità e controllo del rumore espliciti. Gli endpoint amministrativi restano in LAN o sulla rete overlay. Un eventuale stato pubblico espone solo disponibilità aggregata, senza indirizzi interni o identità dei componenti.
