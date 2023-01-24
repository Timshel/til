# Nitter


## Setup

```bash
echo "deb http://deb.debian.org/debian bullseye-backports main contrib non-free" > /etc/apt/sources.list.d/backports.list
apt update
apt install git libsass-dev libpcre3-dev redis-server
apt install -t bullseye-backports nim

adduser nitter --disabled-password --home /home/nitter

su nitter
git clone https://github.com/zedeus/nitter
cd nitter
nimble install -y --depsOnly
nimble build -d:danger -d:lto -d:stri
nimble scss
nimble md
mkdir -p ../workdir
cp -r nitter public ../workdir
cp nitter.example.conf ../workdir/nitter.conf
exit

nano /etc/systemd/system/nitter.service
```

Service file:
```
[Unit]
Description=Nitter (An alternative Twitter front-end)
After=syslog.target
After=network.target

[Service]
Type=simple

# set user and group
User=nitter
Group=nitter

# configure location
WorkingDirectory=/home/nitter/workdir
ExecStart=/home/nitter/workdir/nitter

Restart=always
RestartSec=15

[Install]
WantedBy=multi-user.target
```

Enables service
```
systemctl enable redis-server.service
systemctl enable nitter.service

systemctl start redis-server.service
systemctl start nitter.service
```