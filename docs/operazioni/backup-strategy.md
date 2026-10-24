[Indice della sezione](indice.md) · [Indice](../indice.md)

# Backup

Un disco separato dell’hypervisor è la destinazione generale dei backup.
È distinto dal disco delle VM e dallo storage USB operativo del nodo di rete.
L’attivazione è a richiesta; prima di scrivere viene verificata l’identità del
filesystem per evitare copie accidentali sul disco di sistema.

Sono attivi:

- backup giornaliero consistente del password manager: breve arresto del
  servizio, copia di dati/configurazione, riavvio, checksum e archivi privati;
- backup settimanale della VM applicativa con compressione e conservazione
  delle ultime tre copie riuscite, senza includere il laboratorio sospeso.

Verificati un ripristino isolato del database e l’avvio applicativo del password
manager, oltre all’integrità dell’archivio VM. Il login sulla copia ripristinata
e il ripristino completo della VM non sono stati provati.

La share SMB non è ancora coperta da backup. Anche copie complete delle
configurazioni infrastrutturali e una strategia off-site restano da completare.
Gli archivi attuali hanno permessi restrittivi ma non cifratura a riposo.
Git conserva configurazioni, non i dati applicativi.

Proxmox Backup Server su una macchina separata resta pianificato, non operativo.
La sua capacità, rete, retention e finestra di accensione saranno definite
separatamente. Un singolo disco di backup non copre il guasto dell’intero sito.
