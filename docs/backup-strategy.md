# Backup pianificato

Proxmox Backup Server non è installato e non sono ancora operativi backup
centralizzati, retention o test di restore. La share SMB è storage operativo,
non una copia indipendente. Git conserva configurazioni, non i dati applicativi.

Il progetto futuro prevede un server fisicamente separato per:

- VM applicativa e dati persistenti;
- configurazioni e dati del nodo infrastrutturale;
- file condivisi, con consistenza dei database quando necessario;
- eventuale copia cifrata off-site dei dataset essenziali.

Capacità del datastore, rete, retention e finestra di accensione sono da definire.
Lo spegnimento dovrà attendere il completamento dei task. Il progetto sarà
considerato verificato solo dopo un restore eseguito in ambiente isolato.
