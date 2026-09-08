```bash
rpm-ostree install virt-install libvirt-daemon-config-network libvirt-daemon-kvm qemu-kvm virt-manager virt-viewer

sudo systemctl start libvirtd
sudo systemctl enable libvirtd
```

```bash
minikube start --driver kvm2 --memory 6144 --network-plugin=cni --enable-default-cni --container-runtime=containerd --bootstrapper=kubeadm
```

```bash
minikube start --driver kvm2 --memory 6144 --network-plugin=cni --enable-default-cni --container-runtime=containerd --bootstrapper=kubeadm
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
sudo usermod -aG libvirt $USER

minikube start --driver kvm2 --memory 6144 --network-plugin=cni --enable-default-cni --container-runtime=containerd --bootstrapper=kubeadm --nodes 3
```

```bash
minikube addons enable metrics-server
minikube addons enable headlamp

minikube kubectl -- create token headlamp --duration 24h -n headlamp
```

## Reference

https://minikube.sigs.k8s.io/docs/drivers/kvm2/

https://docs.fedoraproject.org/en-US/quick-docs/virtualization-getting-started/

https://fedoramagazine.org/full-virtualization-system-on-fedora-workstation-30/
