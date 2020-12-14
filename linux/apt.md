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

## Source preference:

Just list in order, cf https://linux.die.net/man/5/apt_preferences

```
Several instances of the same version of a package may be available when the sources.list(5) file contains references to more than one source. In this case apt-get(8) downloads the instance listed earliest in the sources.list(5) file. The APT preferences file does not affect the choice of instance, only the choice of version.
```

Different from apt preferences : https://wiki.debian.org/AptConfiguration


## Policy preferences:

Instead of using hold to keep a specific version (will prevent upgrade).
You can set some policy preferences by creating a file in `/etc/apt/preferences.d` :

```bash
Package: kubectl
Pin: version 1.17.2-*
Pin-Priority: 1000
```