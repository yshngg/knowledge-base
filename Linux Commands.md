## curl

```bash
curl -fsSL ifconfig.me
curl -fsSL ipinfo.io/ip

curl -x http://127.0.0.1:1080 -fsSL ipinfo.io/ip

curl -x https://127.0.0.1:1080 --proxy-cacert ./pki/ca.pem -v --proxy-http2 -I https://example.org
```

## fzf

```bash
# Open in tmux popup if on tmux, otherwise use --height mode

# ~/.bashrc
export FZF_DEFAULT_OPTS='--height 40% --tmux bottom,40% --layout reverse --border top'

# ~/.config/fish/config.fish
set -gx FZF_DEFAULT_OPTS '--height 40% --tmux bottom,40% --layout reverse --border top'
```

## git

```bash
git config --local user.email yshngg@outlook.com
git config --local user.name 'Yusheng'

git config --local http.proxy <proxy>
git config --local https.proxy <proxy>

man gitrevisions # Specifying revisions and ranges for Git
```

## gsetting

```bash
gsettings set org.gnome.system.proxy mode 'manual'
gsettings set org.gnome.system.proxy.socks host 'proxy.local'
gsettings set org.gnome.system.proxy.socks port 1080
gsettings set org.gnome.system.proxy ignore-hosts '["localhost", "127.0.0.0/8", "::1"]'
```
## go

```bash
GODEBUG=http2debug=2,http2xconnect=1 go run .
```

## helm

```bash
helm upgrade --debug -n <namespace> --create-namespace -i <release> <chart>
```

## minikube

```bash
# Cannot be local proxy, shch as 127.0.0.1 or localhost.
# For workaround, please refer to:
# https://github.com/kubernetes/minikube/issues/13897#issuecomment-1166252008
export HTTPS_PROXY=http://<proxy>:1080

# More information, see:
# https://minikube.sigs.k8s.io/docs/handbook/vpn_and_proxy/
export NO_PROXY=localhost,127.0.0.1,10.96.0.0/12,192.168.59.0/24,192.168.49.0/24,192.168.39.0/24

sudo usermod -aG docker $USER && newgrp docker

minikube start --driver=docker --nodes 3
```

## kubectl

```bash
kubectl get --raw "/api/v1/namespaces/<namespace>/services/<service>/proxy/<path>"
kubectl get --raw "/api/v1/nodes/<node>/proxy/<path>"

kubectl debug -n <namespace> <pod> -it --image=busybox:latest -- sh
```

## shell script

```bash
#!/usr/bin/env bash

SCRIPT_DIR=$( cd -- "$( dirname -- "${BASH_SOURCE[0]}" )" &> /dev/null && pwd )
```

### proxy

```bash
#!/usr/bin/env bash

# Set up HTTP proxy
eval mode=$(gsettings get org.gnome.system.proxy mode)
eval host=$(gsettings get org.gnome.system.proxy.https host)
eval port=$(gsettings get org.gnome.system.proxy.https port)
if [[ $mode == 'manual' && -n $host && $port -ne 0 ]]; then
  export HTTPS_PROXY="http://$host:$port"
fi
```

```bash
#!/usr/bin/env fish

eval set mode (gsettings get org.gnome.system.proxy mode)
eval set host (gsettings get org.gnome.system.proxy.https host)
eval set port (gsettings get org.gnome.system.proxy.https port)
if [ $mode = 'manual' ]; and [ -n $host ]; and [ $port -ne 0 ]
    set -gx HTTPS_PROXY "http://$host:$port"
end
```

## sudo

```bash
# when you forget to use sudo with a command
sudo !!
```

## tcpdump

```bash
tcpdump -vv -X -i lo port 8088
```

## tmux

```bash
# Config tmux default shell
cat << EOF >> ~/.tmux.conf
set-option -g default-shell /usr/bin/fish

EOF
```

## vim

xref: https://stackoverflow.com/a/7078429

```vim
" Allow saving of files as sudo when I forgot to start vim using sudo.
cmap w!! w !sudo tee > /dev/null %
```

[50+ Essential Linux Commands: A Comprehensive Guide](https://www.digitalocean.com/community/tutorials/linux-commands)
