## Tasks
- Create subnet in VPC `xgeducation`
```text
1. *Log into the Google Cloud Console**: Make sure you are logged into your GCP account.
    
2. **Select or Create a Project**: If you have an existing project, select it. If not, you can create a new project from the Cloud Console.
    
3. **Navigate to VPC Networks**:
    
    - In the Google Cloud Console, go to the "Navigation menu" (the three horizontal lines in the upper left).
    - Under "Networking," select "VPC network."
4. **Create a VPC Network** (if not already created):
    
    - If you don't have an existing VPC network, you can create one by clicking the "Create VPC network" button.
    - Follow the prompts to configure your VPC network, such as providing a name and description.
5. **Create a Subnet**:
    
    - Once you have a VPC network, select it from the list.
    - In the "VPC network details" page, click on the "Subnets" tab.
6. **Add Subnet**:
    
    - Click the "Create subnet" button.
7. **Configure Subnet**:
    
    - Provide a name for the subnet.
    - Choose the region in which the subnet will be created.
    - Provide an IP address range in CIDR notation (e.g., 10.0.0.0/24). Make sure this range does not overlap with other subnets in the same VPC.
    - You can also configure other settings like flow logs and secondary IP ranges.
8. **Create Subnet**:
    
    - Click the "Create" button to create the subnet.
9. **Verify Subnet Creation**:
    
    - Wait for the subnet creation process to complete. You can check the status in the "Subnets" tab.
```

- Create GKE cluster
- Access GKE cluster
```bash
# Use Cloud Shell
gcloud container clusters get-credentials CLUSTER_NAME --zone ZONE

# Use Google Cloud CLI
## gke-gcloud-auth-plugin is required
gcloud components install gke-gcloud-auth-plugin

gcloud container clusters get-credentials CLUSTER_NAME --zone ZONE

# gcloud container clusters get-credentials cn-li11zhu-cluster-a --zone asia-northeast1-a
```
- 
- Install Google Cloud CLI 
	- https://cloud.google.com/sdk/docs/install?hl=ja#installation_instructions
- Create VM
- Install minikube, kubectl
- Install Pyroscope(helm)
	- binary
	- micro-services

## Glossary
- alias & auto-completion for kubectl
	- linux:  https://spacelift.io/blog/kubectl-auto-completion
	- windows: 
```bash
echo 'Set-Alias -Name k -Value kubectl' >> $PROFILE
echo 'Register-ArgumentCompleter -CommandName k -ScriptBlock $__kubectlCompleterBlock'  >> $PROFILE
kubectl completion powershell >> $PROFILE
```
- Install PowerShell 7
- Trouble shooting K8s
	- **kubectl get** - list resources
	- **kubectl describe** - show detailed information about a resource
	- **kubectl logs** - print the logs from a container in a pod
	- **kubectl exec** - execute a command on a container in a pod
- gcloud interactive shell
  https://cloud.google.com/sdk/docs/interactive-gcloud
- how to refresh bash
```bash
. ~/.bashrc 
```
- gke cluster `cn-li11zhu-cluster-a` zone `asia-northeast1-a`
- Grafana admin user's pw `nEP6DhKSS0lglMLGgmHDNWPOEDJucNmeqM5lYeGR` -> `admin`
- pyroscope helm chart: https://github.com/pyroscope-io/helm-chart
- kubectl switch contexts
```bash
kubectl config get-contexts

kubectl config use-context docker-desktop
kubectl config use-context gke_nttd-platformtec_asia-northeast1-a_cn-li11zhu-cluster-a
```
- kubectl set default namespace
	- https://www.cloudytuts.com/tutorials/kubernetes/how-to-set-default-kubernetes-namespace/
```bash
kubectl get namespaces 
kubectl config set-context --current --namespace=NAMESPACE
```
- helm
	- `helm list -n xxx //List all the releases under namespace xxx`
	- `helm uninstall <release> //delete a release`
- Error`Unable to resolve the current Docker CLI context "default": context "default": context not found`
	- docker context use default
	- https://github.com/kubernetes/minikube/issues/16788
## Questions
- 外部IPが付与されていない、かつ外部からCloud NATへの接続ができないため、アプリの動作確認はどうする？
	- minikube on VM: 難しい
	- GKE: LoadBalancerのデプロイが必要？
- VSCodeからVMへのSSHは踏み台経由しなければならない？
- [Not Solved]Grafana add datasource error: origin not allowed
	- [Persistently add data source](## Optional: Persistently add data source)
	- [Port-forward to local environment?](https://stackoverflow.com/questions/73306003/how-can-i-solve-grafana-port-forwarding-error-on-gke)
- Artifactory RegistryにPushする権限がない
	- ![[AR push issue.png]]