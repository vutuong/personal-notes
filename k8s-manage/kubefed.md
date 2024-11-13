- Merge config file to host cluster, context:
```
kubefedctl join kubernetes2 --host-cluster-name=kubernetes2 --host-cluster-context=kubernetes2 cluster-context=kubernetes2
kubectl -n kube-federation-system get kubefedclusters.core.kubefed.io
KUBECONFIG=/home/ubuntu/.kube/config:tuong-config kubectl config view --merge --flatten > out.txt
kubectl config use-context kubernetes1
```
- Install kubefedctl:
```
sudo snap install kubefedctl --classic
```

```
helm repo add kubefed-charts https://raw.githubusercontent.com/kubernetes-sigs/kubefed/master/charts
helm search kubefed
helm --namespace kube-federation-system upgrade -i kubefed kubefed-charts/kubefed --create-namespace
kubectl get pods -n kube-federation-system
```
```
kubectl config get-contexts
kubectl -n kube-federation-system get kubefedclusters
```
