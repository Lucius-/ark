# FAQ

## When is it appropriate to use Ark instead of etcd's built in backup/restore?

Etcd's restore tooling is good for recovering from data loss in a single etcd cluster, but does not
support more sophisticated restores. If you only want to recover from data loss in a single etcd
cluster, you're likely better off using etcd's backup/restore tooling.

Examples of cases where Ark is useful:

* backing up both Kubernetes resources and persistent volume state
* cluster migrations
* backing up a subset of your Kubernetes resources
* backing up Kubernetes resources that are stored across multiple etcd clusters (for example if you
  run a custom apiserver)

## Will Ark restore my Kubernetes resources exactly the way they were before?

Yes, with some exceptions. For example, when Ark restores pods it deletes the `nodeName` from the
pod so that it can be scheduled onto a new node. You can see some more examples of the differences
in [pod_restorer.go](https://github.com/heptio/ark/blob/master/pkg/restore/restorers/pod_restorer.go)