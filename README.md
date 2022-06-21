## About

This project is a combination of a WireGuard VPN server and PiHole DNS Sinkhole built using Docker Compose. It enables users to easily deploy and manage a Wireguard VPN that can harness to power of PiHole ad blocking remotely.

## Setup

### Docker and Docker Compose

To get started you first need to install Docker and Docker Compose. 
```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### Clone Repository

Next you have to clone and open this repo.

```
git clone https://github.com/sallen222/docker-wireguard-pihole.git
cd docker-wireguard-pihole
```

### Edit Compose File

Once in the directory, open docker-compose.yml.

```
sudo nano docker-compose.yml
```

Here under services:pihole:environment:WEBPASSWORD you should change REPLACE_ME to your desired password.

You should also change the TZ variable in both PiHole and WireGuard to your local time zone and change the value of PEERS to reflect the amount of clients you want to connect to the VPN.

### Port Forwarding

So the wireguard server can receive packets from clients, you need to configure port forwarding so port 51820 on your router is forwarded to port 51820 on the server.

This process is different depending on your router so this might take some research on your part.

### Starting Containers

Now that port forwarding is set up you are ready to start the containers.

While in this repo's directory -

```
sudo docker compose up
```

## Client Setup

Now that the containers are up and running you can set up the wireguard clients.

### Mobile

From the home directory

```
sudo docker exec -it wireguard /app/show-peer 1
```
This command shows the qr code associated with peer1. If you have configured multiple peers, change the last number to the desired peer number.

### Windows

You can connect from a windows client by copying the `PEERNAME.conf` file from `/path/to/appdata/config/PEER_NAME` to the client machine and selecting "Import tunnels from file" in the WireGuard software.

## PiHole Access

While connected to WireGuard, the PiHole can be accessed via http://172.21.0.3/admin

While not connected it can be accessed using your server's IP address.

If you didn't set the password in `docker-compose.yml`, the password is REPLACE_ME

## Split Tunnel Configuration (Recommended)

While using wireguard, your internet speed is bottlenecked by your home wifi's upload speed. To get around this, you can use a Split Tunnel configuration that only sends DNS requests through the tunnel. While this greatly improves internet speeds, your data is no longer protected by the WireGuard tunnel.

Modify your wireguard client `AllowedIps` to `172.21.0.3/32`