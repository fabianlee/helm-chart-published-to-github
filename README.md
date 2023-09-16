# Example of Helm chart published to Github


## Creating default nginx chart
```
mkdir -p charts
cd charts
helm create nginx
cd ..
```

## Installing local chart as release

```
release_name=nginx1
release_ns=default
revision=1

# install chart as release to cluster
helm install --dry-run $release_name charts/nginx/

# show installed releases in cluster
helm list --filter=$release_name
helm history $release_name -ns $release_ns
helm get values $release_name -n $release_ns

# show metadata in secret
kubectl get secret sh.helm.release.v1.$release_name.v$revision -n $release_ns -o json | jq .data.release | tr -d '"' | base64 -d | base64 -d | gzip -d | jq -r '.chart.metadata'

# show pods deployed for nginx chart
kubectl get pods --namespace default -l "app.kubernetes.io/name=nginx,app.kubernetes.io/instance=$release_name"

```
