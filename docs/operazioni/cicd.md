[Indice della sezione](indice.md) · [Indice](../indice.md)

# CI/CD

## Pipeline attiva: documentazione

```text
sorgenti private allowlisted
  -> export e controlli anti-leak
  -> commit sul branch pubblico automatico
  -> Pull Request
  -> review e merge manuali
```

L'output pubblico non è sorgente del privato. I secret GitHub sono forniti
al job senza copiarli nei file esportati. Non si esegue push diretto alla main pubblica.

## Esperienza applicativa precedente

La pipeline applicativa dismessa separava build OCI e deploy: tag Git SHA
immutabile, identità CI temporanea su rete privata, SSH con operazioni
allowlisted e health check. Il rollback selezionava un'immagine nota.
Non è una pipeline di deploy attualmente operativa.

Il prossimo laboratorio Kubernetes/Jenkins servirà a provare nuovamente
build, deploy, aggiornamento e rollback su un'applicazione di test.
