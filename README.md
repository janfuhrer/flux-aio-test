# flux-aio-test

Test with [flux-aio](https://github.com/stefanprodan/flux-aio/tree/main)

1. Install kind without CNI

```bash
kind create cluster --config kind.yaml
```

2. Install flux-aio

```bash
timoni -n flux-system apply flux oci://ghcr.io/stefanprodan/modules/flux-aio
```

3. Install CNI

```bash
kubectl apply -k clusters/prod
```

4. Verify installation (Cilium & CoreDNS)

```bash
kubectl -n flux-system wait helmrelease/cilium --for=condition=ready --timeout=5m
kubectl -n kube-system rollout status deploy/coredns --timeout=5m
```

5. Uninstall flux-aio

```bash
flux -n flux-system uninstall
```

6. Bootstrap fluxcd repo (normal flux installation)