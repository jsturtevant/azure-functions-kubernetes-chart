# Helm Chart for Azure Functions on Kubernetes

## Installing
There are a few settings you should override:

- `resources.requests.cpu` - required for Pod Autoscaler to work
- `image.repository` -  Should be your image with your function app running in it (see http://www.jamessturtevant.com/posts/Running-the-Azure-Functions-runtime-in-kubernetes/ for creating a docker image with the azure functions runtime in it)
- `scale.maxReplicas`
- `scale.minReplicas`
- `scale.cpuUtilizationPercentage`

An example would be:

```bash 
helm install --set resources.requests.cpu=200m \
             --set image.repository=vyta/functions \
             --set scale.maxReplicas=10 \
             --set scale.minReplicas=1 \
             --set scale.cpuUtilizationPercentage=50 \
             ./az-func-k8
```

## Debugging 
If you want to see what install might look like:

```bash
helm install --debug --dry-run --set resources.requests.cpu=200m ./az-func-k8
```