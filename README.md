# etcd-operator

This is a tranformed version of the existing, now archived [etcd-operator](https://github.com/coreos/etcd-operator). Our version support the creation of two same name releases into different namespaces.

How to start?

You could package the chart via the following command: `helm package etcd-operator`

You could install your release in two different namespaces while using the same release name (in our case `my-release`):

`helm install -f etcd-operator/values.yaml my-release etcd-operator --set customResources.createEtcdClusterCRD=true -n namespace1`

`helm install -f etcd-operator/values.yaml my-release etcd-operator --set customResources.createEtcdClusterCRD=true -n namespace2`
