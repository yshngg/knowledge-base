## curl

```bash
curl -fsSL ifconfig.me
curl -fsSL ipinfo.io/ip

curl -x http://127.0.0.1:1080 -fsSL ipinfo.io/ip

curl -x https://127.0.0.1:1080 --proxy-cacert ./pki/ca.pem -v --proxy-http2 -I https://example.org

# download a file
curl -O https://example.com/filename
```

## docker

#### How to install docker-compose on Fedora CoreOS?

```bash
# Just install it using `rpm-ostree`
sudo rpm-ostree install docker-compose

# and then reboot in order for the changes to the OSTree to take effect
sudo systemctl reboot
```

xref: https://techoverflow.net/2021/05/15/how-to-install-docker-compose-on-fedora-coreos/



## fzf

```bash
# open in tmux popup if on tmux, otherwise use --height mode

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

man gitrevisions # specify revisions and ranges for Git
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

## install

```
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

## ip

https://www.cyberciti.biz/faq/linux-ip-command-examples-usage-syntax/

## hostname

```bash
# primary IP address of the local machine
hostname --all-ip-addresses | cut --delimiter ' ' --fields 1
```

## minikube

```bash
# cannot be local proxy, shch as 127.0.0.1 or localhost
# for workaround, please refer to:
# https://github.com/kubernetes/minikube/issues/13897#issuecomment-1166252008
export HTTPS_PROXY=http://$IP:$PORT

# More information, see:
# https://minikube.sigs.k8s.io/docs/handbook/vpn_and_proxy/
export NO_PROXY=localhost,127.0.0.1,10.96.0.0/12,192.168.59.0/24,192.168.49.0/24,192.168.39.0/24

sudo usermod -aG docker $USER && newgrp docker

minikube start --driver=docker --nodes 3
```

```bash
#!/usr/bin/env fish
set -gx HTTPS_PROXY https://(hostname --all-ip-addresses | cut --delimiter ' ' --fields 1):1080
set -gx NO_PROXY localhost,127.0.0.1,10.96.0.0/12,192.168.59.0/24,192.168.49.0/24,192.168.39.0/24

#!/usr/bin/env bash
export HTTPS_PROXY="https://$(hostname --all-ip-addresses | cut --delimiter ' ' --fields 1):1080"
export NO_PROXY=localhost,127.0.0.1,10.96.0.0/12,192.168.59.0/24,192.168.49.0/24,192.168.39.0/24
```
## kubectl

```bash
kubectl get --raw "/api/v1/namespaces/<namespace>/services/<service>/proxy/<path>"
kubectl get --raw "/api/v1/nodes/<node>/proxy/<path>"

kubectl debug -n <namespace> <pod> -it --image=busybox:latest -- sh
```

## kubectl-ai

```bash
# deepseek https://api-docs.deepseek.com/

export OPENAI_API_KEY=<key>
export OPENAI_ENDPOINT=https://api.deepseek.com/v1
kubectl-ai --llm-provider=openai --model=deepseek-chat # or deepseek-reasoner
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

## shell script

```bash
#!/usr/bin/env bash

SCRIPT_DIR=$( cd -- "$( dirname -- "${BASH_SOURCE[0]}" )" &> /dev/null && pwd )
```

## ssh

```bash
ssh -i ".pem" -L <port>:127.0.0.1:<port> -N root@<address>
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
