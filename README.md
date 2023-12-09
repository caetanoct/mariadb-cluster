# MariaDB Cluster

This repository contains configurations and manifests to deploy a MariaDB Galera Cluster in a Kubernetes environment. The setup utilizes Minikube for local development and testing purposes.

## Running The Cluster

```bash
kubectl apply -f manifests/
```

This will deploy a MariaDB Galera Cluster with the specified configurations. The manifests include StatefulSets, Services, and ConfigMaps necessary for a functional Galera Cluster.

## Adding Data to the MariaDB Galera Cluster

To add data to the MariaDB Galera Cluster, you can follow these steps:

1. Connect to the Galera Cluster using the provided MySQL client command:

    ```bash
    mariadb -h <ip> -u <username> -p <password>
    ```

2. Once connected, create a database named "books" and switch to it:

    ```sql
    CREATE DATABASE books;
    USE books;
    ```

3. Create a table named "authors" and insert sample data:

    ```sql
    CREATE TABLE authors (id INT, name VARCHAR(20), email VARCHAR(20));
    INSERT INTO authors (id, name, email) VALUES (1, "caetano", "caet@company.com");
    ```

## Recovering the MariaDB Galera Cluster

In the event of a cluster failure or inconsistency, you can recover the MariaDB Galera Cluster using the following steps:

1. Run the `mariadbd` command with the `--wsrep-recover` option:

    ```bash
    mariadbd --user=mysql --wsrep-recover
    ```

2. Update the `grastate.dat` file to indicate that it is safe to bootstrap:

    ```bash
    sed -i 's/safe_to_bootstrap: 0/safe_to_bootstrap: 1/g' /var/lib/mysql/grastate.dat
    ```
