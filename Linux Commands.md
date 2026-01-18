## curl

```bash
curl -fsSL ifconfig.me

curl -v -x https://127.0.0.1:1080 --proxy-cacert ./pki/ca.pem --proxy-http2 -I https://example.org
```

## git

```bash
git config --local user.email yshngg@outlook.com
git config --local user.name 'Yusheng Guo'

git config --local http.proxy <proxy>
git config --local https.proxy <proxy>
```

## go

```bash
GODEBUG=http2debug=2,http2xconnect=1 go run .
```

## helm

```bash
helm upgrade --debug -n <namespace> --create-namespace -i <release> <chart>
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
eval host=$(gsettings get org.gnome.system.proxy.https host)
eval port=$(gsettings get org.gnome.system.proxy.https port)
if [[ -n ${host} && -n port ]]; then
  export HTTPS_PROXY="http://${host}:${port}"
fi
```

## tcpdump

```
tcpdump -vv -X -i lo port 8088
```

## tmux

```
# Config tmux default shell
cat << EOF >> ~/.tmux.conf
set-option -g default-shell /usr/bin/fish

EOF
```

[50+ Essential Linux Commands: A Comprehensive Guide](https://www.digitalocean.com/community/tutorials/linux-commands)
