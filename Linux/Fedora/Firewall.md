```bash
firewall-cmd --list-ports

sudo firewall-cmd --add-port=port-number/port-type

sudo firewall-cmd --remove-port=port-number/port-type
```

## Examples

```bash
sudo firewall-cmd --add-port=1080/tcp
```

## Reference

https://docs.fedoraproject.org/en-US/quick-docs/firewalld/

https://firewalld.org/
