```bash
sudoedit /etc/systemd/logind.conf
```

```bash
HandleLidSwitch=ignore
```

```bash
sudo systemctl restart systemd-logind
```

## Reference

https://askubuntu.com/a/372616
https://manpages.ubuntu.com/manpages/xenial/man5/logind.conf.5.html
https://man7.org/linux/man-pages/man5/logind.conf.5.html
