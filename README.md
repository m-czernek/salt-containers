# Salt Proxy Challenge

## Execute the Salt Containers

To execute the containers:

* Build the container images:

```
podman build -t salt-master -f Dockerfile.master
podman build -t salt-minion -f Dockerfile.minion 
```

* Create a network for connectivity and DNS resolution:

```
podman network create salt-network 
```

* Start the master salt node:

```
podman run --name salt --network salt-network salt-master
```

* Start the minion node:

```
podman run --name salt-minion --network salt-network salt-minion:proxy
```

Then, you can execute `salt` commands, e.g.:

```
podman exec -it salt bash
salt '*' test.ping
```

Note that:

* The master node should resolve to the `salt` DNS query. Consequently, the preceding commands use the name `salt`.
* The master node auto-accepts any minion connections. This is not suitable for production environments.
* The master node contains configuration and files necessary to execute the proxy code. See the [Dockerfile](./Dockerfile.master) for more information about the full configuration. 
