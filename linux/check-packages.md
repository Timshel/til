# Check packages

Source :

  - https://askubuntu.com/questions/57682/find-and-reinstall-packages-with-corrupted-files-without-breaking-anything


Use `debsums` to check package integrity.

```bash
$ sudo apt-get install debsums
$ sudo debsums_init
$ sudo debsums -c
```

Identify the associated packages :
```bash
$ sudo debsums -c | xargs -rd '\n' -- dpkg -S | cut -d : -f 1 | sort -u
```

One liner to identify and repair :

```bash
$ apt-get install --reinstall $(dpkg -S $(debsums -c) | cut -d : -f 1 | sort -u)
```

Check again after :

```bash
$ sudo debsums -c
```

You can also check the config files (Changes are expected) :

```bash
$ sudo debsums -as
```