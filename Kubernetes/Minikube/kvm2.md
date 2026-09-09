## install virtualization software

```bash
rpm-ostree install virt-install libvirt-daemon-config-network libvirt-daemon-kvm qemu-kvm virt-manager virt-viewer

sudo systemctl start libvirtd
sudo systemctl enable libvirtd
```

https://docs.fedoraproject.org/en-US/quick-docs/virtualization-getting-started/

## start

### add user to libvirt group

```bash
$ minikube start --driver kvm2 --container-runtime=containerd --nodes 3
😄  minikube v1.38.1 on Fedora 44
✨  Using the kvm2 driver based on user configuration

🚫  Exiting due to PR_KVM_USER_PERMISSION: libvirt group membership check failed:
user is not a member of the appropriate libvirt group
💡  Suggestion: Ensure that you are a member of the appropriate libvirt group (remember to relogin for group changes to take effect!)
📘  Documentation: https://minikube.sigs.k8s.io/docs/reference/drivers/kvm2/
🍿  Related issues:
    ▪ https://github.com/kubernetes/minikube/issues/5617
    ▪ https://github.com/kubernetes/minikube/issues/10070
```

```bash
grep -E '^libvirt:' /usr/lib/group | sudo tee -a /etc/group
sudo usermod -aG libvirt $USER
```

https://github.com/kubernetes/minikube/issues/3467#issuecomment-925480224

https://docs.fedoraproject.org/en-US/atomic-desktops/troubleshooting/#_unable_to_add_user_to_group

```bash
$ minikube start --driver kvm2 --container-runtime=containerd --nodes 3
```

## addons

### metrics-server

```bash
$ minikube addons enable metrics-server
💡  metrics-server is an addon maintained by Kubernetes. For any concerns contact minikube on GitHub.
You can view the list of minikube maintainers at: https://github.com/kubernetes/minikube/blob/master/OWNERS
    ▪ Using image registry.k8s.io/metrics-server/metrics-server:v0.8.1
🌟  The 'metrics-server' addon is enabled
```

```bash
minikube kubectl top nodes
minikube kubectl top pods -A
```

### headlamp

```bash
$ minikube addons enable headlamp
❗  headlamp is a 3rd party addon and is not maintained or verified by minikube maintainers, enable at your own risk.
💡  headlamp is maintained by 3rd party (kinvolk.io) for any concerns contact yolossn on GitHub.
    ▪ Using image ghcr.io/headlamp-k8s/headlamp:v0.40.0
💡  To access Headlamp, use the following command:

	minikube service headlamp -n headlamp

💡  To authenticate in Headlamp, fetch the Authentication Token using the following command:

        kubectl create token headlamp --duration 24h -n headlamp

💡  Headlamp can display more detailed information when metrics-server is installed. To install it, run:

	minikube addons enable metrics-server

🌟  The 'headlamp' addon is enabled
```

## Reference

https://minikube.sigs.k8s.io/docs/drivers/kvm2/

https://docs.fedoraproject.org/en-US/quick-docs/virtualization-getting-started/

https://fedoramagazine.org/full-virtualization-system-on-fedora-workstation-30/
