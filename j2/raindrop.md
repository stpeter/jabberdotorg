# Notes for setting up a Raindrop by hand on a Raspberry Pi

Version 0.1, 2020-01-19

## Using default Raspbian

### overall
* switched to python3 as system default
* timedatectl set-timezone America/Denver

### install prosody
* mkdir /run/prosody && chown prosody /run/prosody
* add hostname to config, enable message archiving and for longer (need to run diff from stock config)

### NAT / ports
* added port forwarding 5200-5300 to both NATs (I have two)
* configured lifetime dhcp lease for raindrop

### dns
* cloudflare nameservers for jeremie.com
* used https://github.com/LINKIWI/cloudflare-ddns-client
* added to cron hourly
* enable dnssec

### certs
* pip install certbot (don’t use apt-get certbot, is old)
* pip install certbot-dns-cloudflare
* create ini file w/ cloudflare global api key, set perms go-r
* certbot certonly --dns-cloudflare --dns-cloudflare-credentials /etc/prosody/certs/certbot.ini -d jeremie.com
* prosodyctl --root cert import /etc/letsencrypt/live
* edit /etc/cron/cron.d/certbot to add --deploy-hook "prosodyctl --root cert import /etc/letsencrypt/live"
