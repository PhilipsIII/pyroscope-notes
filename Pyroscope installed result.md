# Pyroscope UI & Grafana

Pyroscope database comes with a built-in UI, to access it from your localhost you can use:

```
kubectl --namespace pyroscope-test port-forward svc/pyroscope 4040:4040
```

You can also use Grafana to explore Pyroscope data.
For that, you'll need to add the Pyroscope data source to your Grafana instance and configure the query URL accordingly.
See https://grafana.com/docs/grafana/latest/datasources/grafana-pyroscope/ for more details.

The in-cluster query URL for the data source in Grafana is:

```
http://pyroscope.pyroscope-test.svc.cluster.local.:4040
```

# Collecting profiles.


The Grafana Agent has been installed to scrape and discover pprof profiles endpoint via pod annotations.

As an example, to start collecting memory and cpu profile using the 8080 port, add the following annotations to your workload:

```
profiles.grafana.com/memory.scrape: "true"
profiles.grafana.com/memory.port: "8080"
profiles.grafana.com/cpu.scrape: "true"
profiles.grafana.com/cpu.port: "8080"
```


To learn more supported annotations, read our guide https://grafana.com/docs/pyroscope/latest/deploy-kubernetes/#optional-scrape-your-own-workloads-profiles

There are various ways to collect profiles from your application depending on your needs.
Follow our guide to setup profiling data collection for your workload:

https://grafana.com/docs/pyroscope/latest/configure-client/