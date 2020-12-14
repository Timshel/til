# NMap tips

## Check DHCP

```bash
$nmap -d  --script broadcast-dhcp-discover -e en5
```