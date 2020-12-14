# Some Troubleshooting

## Find what is locking a programm

Ex: Borg process locked, no info

```bash
$ strace -s 99 -ffp $pid
select(0, NULL, NULL, NULL, {tv_sec=1, tv_usec=0}) = 0 (Timeout)
mkdir("/root/.cache/borg/c34861375922b05f29883d4f4ac53d9e30a5b31bdcf29837c4ebbc156f6a5c39/lock.exclusive", 0777) = -1 EEXIST (File exists)
stat("/root/.cache/borg/c34861375922b05f29883d4f4ac53d9e30a5b31bdcf29837c4ebbc156f6a5c39/lock.exclusive/1165d5f1221a@2485377892357.24-0", 0x7ffd4e6524c0) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/root/.cache/borg/c34861375922b05f29883d4f4ac53d9e30a5b31bdcf29837c4ebbc156f6a5c39/lock.exclusive", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 6
fstat(6, {st_mode=S_IFDIR|0777, st_size=62, ...}) = 0
getdents(6, /* 3 entries */, 32768)     = 104
getdents(6, /* 0 entries */, 32768)     = 0
close(6)                                = 0
select(0, NULL, NULL, NULL, {tv_sec=1, tv_usec=0}) = 0 (Timeout)
mkdir("/root/.cache/borg/c34861375922b05f29883d4f4ac53d9e30a5b31bdcf29837c4ebbc156f6a5c39/lock.exclusive", 0777) = -1 EEXIST (File exists)
stat("/root/.cache/borg/c34861375922b05f29883d4f4ac53d9e30a5b31bdcf29837c4ebbc156f6a5c39/lock.exclusive/1165d5f1221a@2485377892357.24-0", 0x7ffd4e6524c0) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/root/.cache/borg/c34861375922b05f29883d4f4ac53d9e30a5b31bdcf29837c4ebbc156f6a5c39/lock.exclusive", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 6
fstat(6, {st_mode=S_IFDIR|0777, st_size=62, ...}) = 0
getdents(6, /* 3 entries */, 32768)     = 104
getdents(6, /* 0 entries */, 32768)     = 0
close(6)
...
```

Hum we have an issue with lock :)


## Dump dome packets

`-A` display packet content.

```bash
sudo tcpdump -A -i utun1 dst 10.10.1.1 and port 5353
```