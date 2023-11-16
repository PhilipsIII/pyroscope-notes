## Deployment modes
### Monolithic mode
- all components run in a single process
- meant to be used when you _only need one pyroscope instance_

![[monolithic-mode.svg]]
### Microservice mode
- scale out the number of instances(i.e. distributors, ingesters, queriers)
- share a singular backend for storage and querying(i.e. object storage currently using minio)
![[microservices-mode.svg]]

## Components


