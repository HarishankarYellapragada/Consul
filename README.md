# Consul Setup
--------------------
## Step 1:
Install helm on master node

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

## Step 2:
Install consul agent on all nodes (for CentOS) 
```
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum -y install consul
```
```
consul
consul members
```


## Step 3:
Create persistent volumes using pv.yaml (Used 3 pv's for test 3 worker cluster)

Create a path for pv on all workers 

```
/mnt/data/pv0
/mnt/data/pv1
/mnt/data/pv2
```
```
kubectl apply -f pv.yaml
```

# Step 4:
Create a gossip encryption key

```
kubectl create secret generic consul-gossip-encryption-key --from-literal=key=$(consul keygen)
```
To reference, use:
```
global:
  gossipEncryption:
    secretName: consul-gossip-encryption-key
    secretKey: key
```
# Step 5:
To start consul 
```
helm install consul hashicorp/consul -f config.yaml
```
To uninstall consul 
```
helm uninstall consul
```
# Step 6:
To expose ui of consul. tls disabled
```
kubectl port-forward service/consul-server 8500:8500
```
with tls enabled
```
kubectl port-forward service/consul-server 8501:8501
```
https://localhost:8501
