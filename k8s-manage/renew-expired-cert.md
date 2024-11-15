#!/bin/bash

#cat /usr/local/bin/k8s-certs-renew.sh
echo "## Expiration before renewal ##"
/usr/local/bin/kubeadm certs check-expiration

echo "## Renewing certificates managed by kubeadm ##"
/usr/local/bin/kubeadm certs renew all

echo "## Restarting control plane pods managed by kubeadm ##"
/usr/local/bin/crictl pods --namespace kube-system --name 'kube-scheduler-*|kube-controller-manager-*|kube-apiserver-*|etcd-*' -q | /usr/bin/xargs /usr/local/bin/crictl rmp -f

echo "## Updating /root/.kube/config ##"
cp /etc/kubernetes/admin.conf /root/.kube/config

echo "## Waiting for apiserver to be up again ##"
until printf "" 2>>/dev/null >>/dev/tcp/127.0.0.1/6443; do sleep 1; done

echo "## Expiration after renewal ##"
/usr/local/bin/kubeadm certs check-expiration

