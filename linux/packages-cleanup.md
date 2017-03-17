# The following packages have unmet dependencies

When you have installed some crap from testing or somewhere else and you need to cleanup.

To have a better idea of where the trouve comme from :
```
> apt-get install libreoffice-common -o Debug::pkgProblemResolver=true`
```

To list all packages from backports :

```
> aptitude search '~S ~i ~Abackports'`

> dpkg -l |  grep '~bpo'
```

List all available version for a package :
```
> apt-cache show libreoffice-common`
> aptitude show -v libreoffice-core 2>&1|egrep -i 'version|archive'
```

See the priority for package selection :
```
> aptitude versions libreoffice-core
```

Once you cleanup your source list find all packages which do not match the remaining source :
```
> sudo apt-show-versions | grep -v uptodate
```

For installation of a specific version of a package :
```
> apt-get install libstdc++6=4.9.2-10 --reinstall
```
