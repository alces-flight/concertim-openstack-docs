# Concertim private docker registry

## Configuration required to use the registry

Currently, the Concertim docker registry lacks a public IP address, DNS entry and uses a self-signed certificate.  These limitations require the following setup on any machine trying to use the registry.

The registry is available at `10.151.15.51`. Add the following entry to `/etc/hosts`:

```
10.151.15.51 registry.docker.concertim.alces-flight.com
```

Configure docker to accept insecure SSL certificates for this site.

If the file `/etc/docker/daemon.json` doesn't exist, create it with the following content.  If it does exist configure the `insecure-registries` key to include `registry.docker.concertim.alces-flight.com:443`.

```
$ cat /etc/docker/daemon.json 
{
  "insecure-registries": ["registry.docker.concertim.alces-flight.com:443"]
}
```

Download the SSL certificate used by `registry.docker.concertim.alces-flight.com` and save to the right location.

```
sudo mkdir -p /etc/docker/certs.d/registry.docker.concertim.alces-flight.com
(echo | openssl s_client -showcerts -connect registry.docker.concertim.alces-flight.com:443) | sed -n '/BEGIN CERTIFICATE/,/END CERTIFICATE/p' | sudo tee /etc/docker/certs.d/registry.docker.concertim.alces-flight.com/docker_registry.crt
```

Restart the docker daemon to pick up those changes.

```
sudo systemctl restart docker
```

With that done you can now login to the registry.  You can find credentials in
the vault on `flightcenter-gw` under the `concertim` key.

```
docker login registry.docker.concertim.alces-flight.com
```

You can now push and pull images to and from the registry.


## Registry details

The registry runs on a VM on `garycloud`.  The VM name is
`concertim-docker-registry` and its IP address `10.151.15.51`.

It was created by following the tutorial at
https://www.digitalocean.com/community/tutorials/how-to-set-up-a-private-docker-registry-on-ubuntu-22-04
