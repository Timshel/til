# SSH tips

https://blog.trackets.com/2014/05/17/ssh-tunnel-local-and-remote-port-forwarding-explained-with-examples.html

Forward to port 8080 `127.0.0.1:61027` from gpu1 :
```bash
 ssh -nNT -L 8080:127.0.0.1:61027 gpu1
```