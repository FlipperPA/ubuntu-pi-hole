# ubuntu-pi-hole
How to install Pihole on Ubuntu; works with a $4 monthly Digital Ocean droplet.

##

In this example, we'll host the web admin on port 8080. First, install Pihole:

```bash
curl -sSL https://install.pi-hole.net | bash
pihole-FTL --config webserver.port "8080o,443os,[::]:8080o,[::]:443os"
nano /etc/pihole/pihole.toml
# Change web admin PORT to 8080
pihole-FTL --config webserver.port "8080o,443os,[::]:8080o,[::]:443os"
systemctl restart pihole-FTL
systemctl stop systemd-resolved
systemctl disable systemd-resolved
# Uncomment and change DNSStubListener=no in:
nano /etc/systemd/resolved.conf
rm /etc/resolv.conf
echo "nameserver 127.0.0.1" | sudo tee /etc/resolv.conf
systemctl restart systemd-networkd
nano /etc/pihole/pihole.toml
# Change listeningMode to "SINGLE" instead of "LOCAL"
# BE SURE THE FIREWALL IS IN PLACE FIRST! Install `ufw` with port 22 allowed.
systemctl restart pihole-FTL
```

## Allowing an IP access to the Web Interface

```bash
sudo ufw allow from 123.456.789.10 to any port 8080 proto tcp
```

## Allowing a Class D Range Access to DNS

```bash
ufw allow from 108.4.212.0/24 to any port 53 proto udp
ufw allow from 108.4.212.0/24 to any port 53 proto tcp
```

## Allowing a Class C Range Access to DNS

```bash
ufw allow from 108.4.0.0/16 to any port 53 proto udp
ufw allow from 108.4.0.0/16 to any port 53 proto tcp
```
