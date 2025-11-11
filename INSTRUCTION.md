## Apply Manifests

```bash
# Create namespace
kubectl apply -f .infrastructure/namespace.yml

# Create PersistentVolume
kubectl apply -f .infrastructure/pv.yml

# Create PersistentVolumeClaim
kubectl apply -f .infrastructure/pvc.yml

# Create the ClusterIP service
kubectl apply -f .infrastructure/clusterIp.yml

# Create ConfigMap
kubectl apply -f .infrastructure/configMap.yml

# Create Secret
kubectl apply -f .infrastructure/secret.yml

# Create Horizontal Pod Autoscaler
kubectl apply -f .infrastructure/hpa.yml

# Create Deployment
kubectl apply -f .infrastructure/deployment.yml
```

Check that everything is running:

```bash
kubectl get pods -n todoapp
kubectl get svc -n todoapp
kubectl get pvc -n todoapp
kubectl get pv -n todoapp
```

✅ Expected result:

A pod from the Deployment is in Running state
PVC is Bound
PV is Bound

## Validate that the application is running

Forward the ClusterIP service and test access:

```bash
kubectl port-forward svc/todoapp-service 8081:80 -n todoapp
```

In a separate terminal:

```bash
curl localhost:8081
```

Expected result: Application home page or API response.

Stop forwarding (Ctrl + C) after testing.

## Validate ConfigMap is mounted as files

1) Connect to the running pod:

```bash
kubectl -n todoapp exec -it <todoapp-pod-name> -- sh
```

2) List files in the /configs directory (or wherever you mounted it):

```bash
ls /configs
```

Expected result: Files matching keys from ConfigMap:

```text
PYTHONUNBUFFERED
```

3) Check contents of the file:

```bash
cat /configs/PYTHONUNBUFFERED
```

Expected output:

```text
1
```

4) Exit the pod:

```bash
exit
```

## Validate that the application is running

Forward the ClusterIP service and test access:

```bash
kubectl port-forward svc/todoapp-service 8081:80 -n todoapp
```

In a separate terminal:

```bash
curl localhost:8081
```

Expected result: Application home page or API response.

Stop forwarding (Ctrl + C) after testing.

## Validate Secret is mounted as a file

1) Connect to the running pod:

```bash
kubectl -n todoapp exec -it <todoapp-pod-name> -- sh
```

2) List files in the secrets directory:

```bash
ls /secrets
```

Expected result:

```text
SECRET_KEY
```

3) View the secret value:

```bash
cat /secrets/SECRET_KEY
```

Expected output:
The decoded value of your SECRET_KEY

4) Exit the pod:

```bash
exit
```

## Cleanup

```bash
kubectl delete -f .infrastructure/deployment.yml
kubectl delete -f .infrastructure/hpa.yml
kubectl delete -f .infrastructure/secret.yml
kubectl delete -f .infrastructure/configMap.yml
kubectl delete -f .infrastructure/clusterIp.yml
kubectl delete -f .infrastructure/pvc.yml
kubectl delete -f .infrastructure/pv.yml
kubectl delete -f .infrastructure/namespace.yml
```