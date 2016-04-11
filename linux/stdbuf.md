# stdbuf: Modify buffer handling for streams

The default size is based on the page size (oftern 4096), It can make the stream look non responsive.
```
getconf PAGESIZE
```

MODE is 'L' the corresponding stream will be line buffered.

```
stdbuf -oL ansible-playbook -v  ... | logger
```

Unbuffered :
```
stdbuf -o0 ansible-playbook -v ... | logger
```

Other options such as custom buffer size are available.