# etcd-operator

This is a tranformed version of the existing, now archived [etcd-operator](https://github.com/coreos/etcd-operator).

The difference between the legacy version and ours in the following: We support **same name release** within a cluster in different namespaces

What does it mean concretly?

Supposing you add the legacy etcd-cluster `helm repo add stable https://kubernetes-charts.storage.googleapis.com/` and then try to deploy two releases named `my-release` in `namespace1` (`helm install my-release stable/etcd-operator -n namespace1`) and `namespace2` (`helm install my-release stable/etcd-operator -n namespace2`).

The first one will succeed, but you'll get the following error for the second namespace:

>Error: INSTALLATION FAILED: rendered manifests contain a resource that already exists. Unable to continue with install: ClusterRole "my-release-etcd-operator-etcd-operator" in namespace "" exists and cannot be imported into the current release: invalid ownership metadata; annotation validation error: key "meta.helm.sh/release-namespace" must equal "namespace2": current value is "namespace1"

There is in fact a collision between `clusterrole` and `clusterrolebinding`.

Our version is fixing this issue by using a different naming structure for these resources.

Here is how you install ours until we get it hosted:
- Clone this repository.
- Run the following command to deploy `helm install -f etcd-operator/values.yaml <you-release-name> etcd-operator --set customResources.createEtcdClusterCRD=true -n <your-namespace>`

You can also package the repo: `helm package etcd-operator`
