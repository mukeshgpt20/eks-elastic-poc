Install Helm Chart
```
curl -LO https://get.helm.sh/helm-v3.19.0-linux-amd64.tar.gz
tar -zxvf helm-v3.19.0-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm
helm version
```
aws eks update-kubeconfig --region <region> --name <cluster-name>

2. Make sure EBS CSI driver is installed
   aws eks describe-addon \
  --cluster-name <CLUSTER_NAME> \
  --addon-name aws-ebs-csi-driver
   
  aws eks create-addon \
  --cluster-name <CLUSTER_NAME> \
  --addon-name aws-ebs-csi-driver

3. kubectl get pods -n kube-system | grep ebs-csi
4. vi gp3-storageclass.yaml
   ```
   apiVersion: storage.k8s.io/v1
kind: StorageClass8.
metadata:
  name: gp3
  annotations:
    storageclass.kubernetes.io/is-default-class: "true"

provisioner: ebs.csi.aws.com

volumeBindingMode: WaitForFirstConsumer

allowVolumeExpansion: true

parameters:
  type: gp3
  encrypted: "true"
  fsType: ext4

   ```

5. kubectl apply -f gp3-storageclass.yaml
6. kubectl get storageclass
7. helm repo add elastic https://helm.elastic.co
8. helm repo update
9. helm search repo elastic/eck-operator
10. helm show values elastic/eck-operator
11. helm install elastic-operator \
  elastic/eck-operator \
  -n elastic-system \
  --create-namespace
12. kubectl get pods -n elastic-system
13. Create an Elasticsearch namespace



