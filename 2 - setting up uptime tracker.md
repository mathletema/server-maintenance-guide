# Setting up uptime tracker

Going to use kuna for this. Clean UI and looks good!

### Installing docker

from docker's docs:

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo "deb [arch=$(dpkg --print-architecture) \
    signed-by=/etc/apt/keyrings/docker.asc] \
    https://download.docker.com/linux/debian \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Install

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Installing kuna

```bash
sudo docker run -d --restart=always -p 3001:3001 -v uptime-kuma:/app/data --name uptime-kuma louislam/uptime-kuma:1
```

### Setting it up

visit http://vps-xxxxxxxx.vps.ovh.us:3001 and create admin account.

add monitors

create status page with slug `/status/default`. You should now have a working status page at http://vps-xxxxxxxx.vps.ovh.us:3001/status/default.

### Setting up nginx

Add `A` Name entries in cloudflare to point to vps ip. Then add this to `/etc/nginx/sites-enabled/ishankagrawal.com`:

```sql
server {
    listen 80;
    listen [::]:80;

    server_name "status.ishankagrawal.com";

    location / {
        proxy_pass "http://localhost:3001/";
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
    }
}
```

### Set root domain to point to default status page

set domain in sidebar to be `status.ishankagrawal.com`.

### Cache

sometimes chrome will cache objects. I had to hard refresh (cmd shift r) to get site to work properly.

### References

1. https://docs.docker.com/engine/install/debian/
2. https://github.com/louislam/uptime-kuma