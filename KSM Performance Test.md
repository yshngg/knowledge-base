[[kvm2]]

```bash
minikube start --driver kvm2 --nodes 3 --extra-config=apiserver.feature-gates=ShardedListAndWatch=true
```
