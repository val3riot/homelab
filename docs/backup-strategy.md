# Strategia di backup

Il design usa un `backup-server` fisicamente indipendente con Proxmox Backup Server. Separare lo storage di backup dall'hypervisor di produzione protegge dal guasto del singolo host o disco di sistema.

## Livelli di backup

- I backup VM proteggono l'intera unità di ripristino `docker-host`.
- I backup del nodo infrastrutturale proteggono configurazioni e dati persistenti.
- I database producono dump consistenti prima del backup filesystem quando necessario.
- I dataset più importanti sono candidati a una futura copia cifrata off-site.

Il server può accendersi per una finestra controllata, attendere il datastore, ricevere i job, verificare i risultati, applicare retention e garbage collection e spegnersi soltanto a task conclusi. Si evita intenzionalmente uno spegnimento a orario fisso.

La retention combina punti giornalieri, settimanali e mensili ed è adattata alla capacità. La verifica controlla l'integrità; soltanto i test di restore dimostrano la recuperabilità. Le prove periodiche ripristinano una VM o dati selezionati in un ambiente isolato e registrano l'esito.

Una file share è storage operativo, non un backup. I dati condivisi ricevono una copia protetta indipendente con retention propria.
