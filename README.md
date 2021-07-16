# Consul Setup
--------------------
## Step 1:
Install helm on 
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh

## Step 2:
Create persistent volumes using pv.yaml (Used 3 pv's for test 3 worker cluster)

Create a path for pv on all workers 

```
/mnt/data/pv0
/mnt/data/pv1
/mnt/data/pv2
```
## Step 3:
Create a gossip encryption key

```
kubectl create secret generic consul-gossip-encryption-key --from-literal=key=$(consul keygen)
```
