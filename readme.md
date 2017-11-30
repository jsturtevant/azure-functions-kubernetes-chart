# Helm Chart for Azure Functions on Kubernetes

## Installing
There are a few settings you should override:

- `functionApp.name` - name of your function apps your using
- `resources.requests.cpu` - required for Pod Autoscaler to work
- `image.repository` -  Should be your image with your function app running in it (see http://www.jamessturtevant.com/posts/Running-the-Azure-Functions-runtime-in-kubernetes/ for creating a docker image with the azure functions runtime in it)
- `scale.maxReplicas` - max number of replicas
- `scale.minReplicas` - min number of replicas
- `scale.cpuUtilizationPercentage` - cpu percentage

An example would be:

```bash 
helm install --set functionApp.name=sampleapp \
             --set resources.requests.cpu=200m \
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

## Test the scaling
Test that the scaling works:

```bash
docker run -it busybox /bin/sh
#
\#: 		while true; do wget -q -O- http://<your-ipaddress>/api/httpfunction?name=testingload; done 
```

In a separate terminal run:

```bash
kubectl get hpa
kubectl get deploy <deployment-name>
```