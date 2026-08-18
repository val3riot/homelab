# CI/CD

Il modello di delivery separa la build applicativa dal deploy specifico dell'ambiente.

```text
commit sorgente
  → test
  → build immagine OCI
  → tag immutabile con Git SHA
  → workflow di deploy
  → identità temporanea sulla rete privata
  → operazione di deploy ristretta
  → verifica health
```

Le immagini sono identificate dallo SHA Git completo: l'artefatto in esecuzione è tracciabile e non dipende da tag di produzione mutabili. Un deploy seleziona una revisione esatta; il rollback seleziona una revisione precedente nota, senza ricostruirla.

I segreti runtime sono forniti dall'ambiente di destinazione e non vengono inclusi nelle immagini o nei file Compose. Le credenziali CI risiedono nei secret GitHub e il runner riceve soltanto i permessi richiesti dal job corrente.

Gli stack applicativi restano indipendenti: una pipeline modifica solo il proprio progetto, valida la configurazione, scarica le immagini selezionate, avvia i servizi e ne controlla lo stato. Un'approvazione manuale può proteggere l'ambiente senza modificare l'artefatto.

La pipeline della documentazione pubblica applica lo stesso modello di revisione: esporta un'allowlist privata, verifica l'assenza di riferimenti riservati, aggiorna un branch automatico e apre una Pull Request. Non aggiorna mai direttamente la `main` pubblica.
