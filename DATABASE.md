# Configuring a database

## Local with PostgreSQL

For local development a `docker compose` database instance will be started using `spring-boot-docker-compose`. 


## Using PostgreSQL as the database with Tanzu Application Platform (TAP)

Using the `config/workload.yaml` it is possible to build, test and deploy this application onto a
Kubernetes cluster that is provisioned with Tanzu Application Platform (https://tanzu.vmware.com/application-platform).

As with the local deployment a PostgreSQL instance needs to be available at the cluster.

1. App Operator Tasks

   - Create the service `ClassClaim` to be consumed by your workload that references your PostgreSQL instance:

      ```bash
      $ tanzu service class-claim create customer-database --class postgresql-unmanaged -n <workload-namespace>
      ```

2. App Developer Tasks

   Now that we have the database instance and class claim configured, we can check the database state by running:
   
   ```bash
   tanzu services class-claims get customer-database -n <workload-namespace>
   ```

   Make sure the claim status is "Ready".
   
   As soon as the database claim is ready, the `Workload` can be deployed.

## Notes

The claim must match the workload.yaml
```yaml
  serviceClaims:
    - name: database
      ref:
        apiVersion: services.apps.tanzu.vmware.com/v1alpha1
        kind: ClassClaim
        name: customer-database
```

### Creating a claim notes
This is creating a claim for a redis database called `sensor-redis`

1. catalog -> `tanzu services classes list`
2. claim -> `tanzu services class-claim create sensor-redis --class redis-unmanaged`
3. claim status -> `tanzu services class-claims get sensor-redis --namespace apps`
4. get claim -> `tanzu services class-claims get sensor-redis`