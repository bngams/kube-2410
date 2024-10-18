# Install 

With :
- Helm
- [Operators](https://dev.mysql.com/doc/mysql-operator/en/mysql-operator-installation-kubectl.html)

# Install via Helm
```
helm repo add mysql-operator https://mysql.github.io/mysql-operator/
helm repo update
helm install my-mysql-operator mysql-operator/mysql-operator \
   --namespace mysql-operator --create-namespace
```



# Install with Operators

Apply CRDs

```
kubectl apply -f https://raw.githubusercontent.com/mysql/mysql-operator/trunk/deploy/deploy-crds.yaml
```

Apply RABCs

```
kubectl apply -f https://raw.githubusercontent.com/mysql/mysql-operator/trunk/deploy/deploy-operator.yaml
```

Check

```
kubectl get deployment mysql-operator --namespace mysql-operator
```

# Deploy a cluster


Create secret
```
kubectl create secret generic mypwds \
        --from-literal=rootUser=root \
        --from-literal=rootHost=% \
        --from-literal=rootPassword="sakila"
```


