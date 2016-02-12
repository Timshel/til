# Make a file immutable

To make a file immutable, as root :
```
chattr +i file
```

To see the immutable flag of files:
```
lsattr
```