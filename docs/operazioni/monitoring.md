[Indice della sezione](indice.md) · [Indice](../indice.md)

# Monitoraggio

Uptime Kuma sul nodo di rete fornisce controlli di disponibilità.
Health dei container, stato systemd e log nativi servono alla diagnosi;
una dashboard non sostituisce il test dell'accesso autenticato o del percorso remoto.

Lo storage USB della share è monitorato con SMART. Il daemon evita di svegliare
un disco già in standby; il comportamento definitivo dopo attività SMB resta
da verificare. Test di copia bidirezionale e accesso VPN sono riusciti.

Metriche centralizzate, Prometheus/Grafana e monitoraggio dei futuri job PBS
non sono ancora implementati. Gli endpoint amministrativi restano protetti.
