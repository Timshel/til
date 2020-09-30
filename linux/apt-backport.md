# For example to backport nodejs from testing

In `/etc/apt/sources.list` (Add the testing source)

    # jessie testing
    deb http://debian.mirrors.ovh.net/debian/ stretch main
    deb-src http://debian.mirrors.ovh.net/debian/ stretch main

Int `/etc/apt/apt.conf` (Keep stable as the default)

    APT::Default-Release "stable";

Check that by default we stay on stable : `apt-cache policy nodejs`.

Install nodejs selecting the testing source : `apt-get install nodejs -t testing`

# Search available package with version

```bash
apt-cache policy libcudnn7
```