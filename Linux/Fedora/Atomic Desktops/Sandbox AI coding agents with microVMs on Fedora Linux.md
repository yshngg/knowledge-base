## Install krun Runtime

```bash
rpm-ostree install crun-krun
```

```bash
systemctl reboot
```

Verifying krun Mode

```bash
podman run --runtime krun alpine uname -a
```

## Run Codex

```bash
# Must login before pulling docker harden images.
podman login dhi.io
podman run --runtime=krun --rm -it dhi.io/codex:latest

# podman run --runtime=krun --rm -it ghcr.io/openai/codex-universal:latest
```

## References

[Sandbox AI coding agents with microVMs on Fedora Linux](https://fedoramagazine.org/sandbox-ai-coding-agents-with-microvms-on-fedora-linux/)

https://hub.docker.com/hardened-images/catalog/dhi/codex

https://github.com/openai/codex-universal
