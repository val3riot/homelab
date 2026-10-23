[Indice della sezione](indice.md) · [Indice](../indice.md)

# Architettura

| Componente | Ruolo |
|---|---|
| Hypervisor Proxmox | ciclo di vita e dischi VM |
| VM Debian Docker | tool personali in progetti Compose separati |
| Nodo di rete | DNS, Nginx, Tailscale, tunnel, Samba e monitoraggio |
| VM laboratorio | base Debian creata; Kubernetes/Jenkins da configurare |
| Workstation GPU | Ollama; disponibile quando la workstation è accesa |
| Server backup | pianificato, non operativo |

```text
LAN / overlay privata -> DNS locale -> Nginx -> tool sulla VM Docker
                                             -> UI AI -> workstation GPU
Internet -> edge identity-aware -> tunnel -> origin selezionati
Nodo di rete -> storage USB -> share SMB
```

Le route web remote usano ancora origin diretti; la convergenza sul proxy è
un progetto separato. Il guasto del nodo di rete interrompe DNS, ingress,
accesso remoto e share, anche se i container della VM restano vivi.

Il desired state è versionato. Ansible usa inventory generata e toolchain
locale riproducibile: check/diff -> review -> apply autorizzato -> validazione
-> controllo convergenza. Gli snapshot runtime sono evidenza separata.

Una VM laboratorio separa gli esperimenti dai tool stabili; la base Debian
è creata, mentre Kubernetes/Jenkins sono ancora da configurare. Backup PBS e cifratura SOPS/age sono ancora da implementare.
