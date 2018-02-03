# Certbot DNS Cloudflare for Kubernetes
[![GitHub forks](https://img.shields.io/github/forks/roytharpe/certbot-dns-cloudflare.svg?style=social&label=Fork&style=flat-square)](https://github.com/roytharpe/certbot-dns-cloudflare)
[![Docker Pulls](https://img.shields.io/docker/pulls/roytharpe/certbot-dns-cloudflare.svg)](https://hub.docker.com/r/roytharpe/certbot-dns-cloudflare/)
[![](https://images.microbadger.com/badges/image/horjulf/certbot-cloudflare.svg)](https://microbadger.com/images/roytharpe/certbot-dns-cloudflare)

This image provides the official Certbot utility with the Cloudflare DNS plugin builtin.

## Usage

- Create local directories where lets encrypt files(keys, certs, renewal config, etc) and logs will be stored.

- Create a .ini file with Cloudflare API credentials:
```
dns_cloudflare_email=<cloudflare_email>
dns_cloudflare_api_key=<cloudflare_api_key>
```

- Run/create the container with the added volumes and the certbot arguments:
```
docker run --interactive --tty \
-v </path/to/letsencrypt_files>:/etc/letsencrypt \
-v </path/to/letsencrypt_logs>:/var/log/letsencrypt \
-v </path/to/cloudflare_creds>:/etc/cloudflare.ini:ro \
horjulf/certbot-cloudflare \
<certbot_args>
```

### Examples

- Request a certificate:
```
docker run --interactive --tty \
-v </path/to/letsencrypt_files>:/etc/letsencrypt \
-v </path/to/letsencrypt_logs>:/var/log/letsencrypt \
-v </path/to/cloudflare_creds>:/etc/cloudflare.ini:ro \
horjulf/certbot-cloudflare \
certonly --dns-cloudflare --dns-cloudflare-credentials /etc/cloudflare.ini -m <admin_email@example.domain> --agree-tos --no-eff-email -d <example.domain>
```

- Renew existing certificates:
```
docker run --interactive --tty \
-v </path/to/letsencrypt_files>:/etc/letsencrypt \
-v </path/to/letsencrypt_logs>:/var/log/letsencrypt \
-v </path/to/cloudflare_creds>:/etc/cloudflare.ini:ro \
horjulf/certbot-cloudflare \
renew --dns-cloudflare-credentials /etc/cloudflare.ini
```

- Cron usable container for renewal:
```
docker create \
--name=certbot_cloudflare_renew
-v </path/to/letsencrypt_files>:/etc/letsencrypt \
-v </path/to/letsencrypt_logs>:/var/log/letsencrypt \
-v </path/to/cloudflare_creds>:/etc/cloudflare.ini:ro \
horjulf/certbot-cloudflare \
renew --dns-cloudflare-credentials /etc/cloudflare.ini --quiet
```
```
docker start -a certbot_cloudflare_renew
```
