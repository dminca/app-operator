# Kubernetes Operator
> Built this thingy to get a grasp on [`operator-sdk`][1] & [Go][2].

## Description

Used the [operator-sdk][1] to generate all this scaffold code in [Go][2].

Chosen Golang because I want to learn more about what this wonderful programming
language can offer.

## Workflow

- start a Kubernetes cluster with [`kind`][3]

```sh
kind create cluster --config tests/kind/config.yaml

# export the KUBECONFIG variable to have access to the K8s cluster
export KUBECONFIG="$(kind get kubeconfig-path --name="kind")"

# is it working?
kubectl cluster-info
```

- apply the K8s stuff

```sh
# create ServiceAccount
kubectl create -f deploy/service_account.yaml

# setup RBAC
kubectl create -f deploy/role.yaml
kubectl create -f deploy/role_binding.yaml

# setup CRD
kubectl create -f deploy/crds/app_v1alpha1_appservice_crd.yaml

# deploy app-operator
kubectl create -f deploy/operator.yaml

# Create an AppService CR
# The default controller will watch for AppService objects and create a pod for each CR
kubectl create -f deploy/crds/app_v1alpha1_appservice_cr.yaml

# Verify that a pod is created
kubectl get pod -l app=example-appservice

# Cleanup
kubectl delete -f deploy/crds/app_v1alpha1_appservice_cr.yaml
kubectl delete -f deploy/operator.yaml
kubectl delete -f deploy/role.yaml
kubectl delete -f deploy/role_binding.yaml
kubectl delete -f deploy/service_account.yaml
kubectl delete -f deploy/crds/app_v1alpha1_appservice_crd.yaml
```

### What does it do?
ATM this Operator is not so useful - it's just a bogus operator. But in future
it'll do _something_ eventually, still need to decide on the scope.

## FAQ
TBD

## Troubleshoot
- `Error: a cluster with the name "kind" already exists`

> Sometimes you get this error if you created a cluster with `kind` before and
eventhough you deleted it the _thingy_ is still there.
To solve this, just execute the _delete_ command again: `kind delete cluster`
and now you can proceed creating the K8s cluster.

[1]: https://github.com/dminca/operator-sdk
[2]: https://golang.org/doc/
[3]: https://kind.sigs.k8s.io/