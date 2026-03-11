
# AWX Deploy - Single Node

Playbook for AWX deployment on a single node with k3s

Please take a look at the original project for more deployment options :

- [Awx-Operator](https://github.com/ansible/awx-operator)

- [Ansible-Awx](https://github.com/ansible/awx)

## Author

 - [DcMcIx](https://www.github.com/DcMcIx)
## Deployment

To deploy AWX make sure you have ansible installed and your inventory populated with the target server :

Playbook execution: 

```bash
ansible-playbook AWX_Deploy_On_Single_Node_K3S.yml
```
You can target your server by adding '--limit' flag :
```bash
ansible-playbook AWX_Deploy_On_Single_Node_K3S.yml --limit [SERVER_IP_OR_SERVER_NAME]
```

## Upgrade AWX-Operator

To Upgrade AWX to the latest release :

```bash
ansible-playbook AWX_Upgrade_Single_Node_K3S.yml
Note: All your data will be preserved (Postgres DB and Persistant volumes won't be wiped) 
```

## Uninstall AWX-Operator

To UNdeploy AWX :

```bash
ansible-playbook AWX_UNDEPLOY_Single_Node_K3S.yml
Note : All AWX Deployment and K3S will be deleted 
```

## Custom AWX-EE
You can use my custom EE (Including community.general and many other collections)

AWX-EE with Ansible-Core 2.20.3 (Py3.14) :
```bash
docker.io/dcmcix/dcmcix-awx-ee:latest
OR
quay.io/dcmcix/dcmcix-awx-ee:latest
```
AWX-EE with Ansible-Core 2.19.1 (Py3.14) :
```bash
docker.io/dcmcix/dcmcix-awx-ee:2.19.1
OR
quay.io/dcmcix/dcmcix-awx-ee:2.19.1
```
## FAQ

#### No access to GUI after deployment :

You must wait a few minutes before having all the pods and services started, depending on your hardware configuration
```
Example: 2vCPU and 4Gi of memory you will need to wait up to 15 minutes to get all started
```
#### How To access AWX GUI :

During the deployment process NodePort is displayed througt the play log

http://{{SERVER_IP}}:{{NODEPORT}} 

Default NodePort : 30080

#### How to get NodePort after the deployment
On the AWX-Operator Server :
```
kubectl config set-context --current --namespace=awx
kubectl describe svc awx-service | grep -i NodePort | grep -v Type
```
#### How To retreive the GUI password :
```
kubectl config set-context --current --namespace=awx
kubectl get secret awx-admin-password -o jsonpath="{.data.password}" | base64 --decode
```
#### Is the server exposed to the network/internet :
Yes, once the AWX-Operator is installed the server will be reachable from the same local network area

And NO, the server won't be exposed to the internet unless you do so by exposing the IP/NodePort

#### Is this deployment suitable for a "Production" environment :
Note that no support is provided for this deployment (this is a personal project that I wanted to share based on the original awx/awx-operator project). If you need a reliable production environment with support, please consider looking at :

 - [RedHat Ansible Automation Platform](https://www.redhat.com/en/technologies/management/ansible)
 - [Ansible](https://www.ansible.com)
