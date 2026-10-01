```bash
$ curl -O https://pkgs.tailscale.com/stable/fedora/tailscale.repo
[tailscale-stable]
name=Tailscale stable
baseurl=https://pkgs.tailscale.com/stable/fedora/$basearch
enabled=1
type=rpm
repo_gpgcheck=1
gpgcheck=0
gpgkey=https://pkgs.tailscale.com/stable/fedora/repo.gpg
$ sudo install -o 0 -g 0 -m644 tailscale.repo /etc/yum.repos.d/tailscale.repo
```

```bash
# Install Tailscale
rpm-ostree install tailscale
# Enable and start tailscaled
sudo systemctl enable --now tailscaled
# Start Tailscale!
sudo tailscale up
```

## Reference

https://tailscale.com/
https://github.com/tailscale
https://pkgs.tailscale.com/stable/
