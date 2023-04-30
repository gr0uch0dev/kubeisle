# Enable Docker Metrics
To guarantee observability of your Docker, it is essential to enable the metrics. This will allow to use tool like Prometheus-AlertManager-Grafana to understand what is happening behind the cluster.

Exporting the metrics is super straightforward, and requires only a modification of the `/etc/docker/daemon.json`.

Do the following modification in order t

```
{
  "metrics-addr" : "127.0.0.1:9323"
}
```

Note: if you're exposing the docker socket, you must modify the `127.0.0.1` with the IP address of the docker socket inside your network. Best way is probably to go with the address of the `docker0` interface.

Once you're done, docker must be rebooted. On Fedora it is possible to do is easily in the following two ways (they're the same)

```
service docker restart # option 1
systemctl restart docker # option 2
```

Once done, Prometheus can be used to gather the data required to allow you to make your queries.

## Configure Prometheus

Prometheus has a `prometheus.yml` file to configure it.

```
  - job_name: 'docker'
         # metrics_path defaults to '/metrics'
         # scheme defaults to 'http'.

    static_configs:
      - targets: ['same-ip-as-before:9323']
```


Reference: https://docs.docker.com/config/daemon/prometheus/