# ci-cd-demo-run
My goal in this demo run is to create an local infrastructure that will install from scratch to meet the ci/cd needs of an application. Since it is an educational purpose, it has been prepared without any security concerns. Almost all endpoints run as http, with all username and passwords left by default. **If you're looking for something suitable for use in live systems and you're in the way of this repo, _don't use it_ (at least for now).**
No changes have been made on the source code ([simple-java-maven-app](https://github.com/jenkins-docs/simple-java-maven-app)) of the sample application, only the Jenkinsfile has been edited and the Dockerfile and Helm Charts have been added.

## Tech Stack
* Ansible
* Calico
* Gitea
* Helm
* Jenkins
* Kubernetes (1.18.12)
* Local Path Provisioner
* MetalLB
* Nexus
* Nginx Ingress
* Vagrant
* Oracle VirtualBox

## Installation Steps
##### Create VMs, install kubernetes and helm
```bash
cd Vagrant
vagrant up
```

##### Deploy MetalLB
```bash
cd .. 
ansible-playbook -i 192.168.6.150, Ansible/kubernetes-after-install/deploy-metallb.yml --key-file Vagrant/.vagrant/machines/k8s-master/virtualbox/private_key
```

##### Depoloy Nginx Ingress Controller
```bash
ansible-playbook -i 192.168.6.150, Ansible/kubernetes-after-install/deploy-nginx-ingress.yml --key-file Vagrant/.vagrant/machines/k8s-master/virtualbox/private_key
```

##### Deploy Nexus
```bash
ansible-playbook -i 192.168.6.150, Ansible/kubernetes-after-install/deploy-nexus.yml --key-file Vagrant/.vagrant/machines/k8s-master/virtualbox/private_key
```

##### Deploy Jenkins
```bash
ansible-playbook -i 192.168.6.150, Ansible/kubernetes-after-install/deploy-jenkins.yml --key-file Vagrant/.vagrant/machines/k8s-master/virtualbox/private_key
```

##### Deploy Gitea
```bash
ansible-playbook -i 192.168.6.150, Ansible/kubernetes-after-install/deploy-gitea.yml --key-file Vagrant/.vagrant/machines/k8s-master/virtualbox/private_key
```

##### Set Insecure Registry
```bash
ansible-playbook -i 192.168.6.150, Ansible/kubernetes-after-install/setup-insecure-registry.yml --key-file Vagrant/.vagrant/machines/k8s-master/virtualbox/private_key
ansible-playbook -i 192.168.6.151, Ansible/kubernetes-after-install/setup-insecure-registry.yml --key-file Vagrant/.vagrant/machines/node-1/virtualbox/private_key
ansible-playbook -i 192.168.6.152, Ansible/kubernetes-after-install/setup-insecure-registry.yml --key-file Vagrant/.vagrant/machines/node-2/virtualbox/private_key
```
*(To do: add inventory file)*

## Helm Chart Sources
* [Gitea](https://github.com/jfelten/gitea-helm-chart)
* [Jenkins](https://github.com/jenkinsci/helm-charts/tree/main/charts/jenkins)
* [Local Path Provisioner](https://github.com/rancher/local-path-provisioner/tree/master/deploy/chart)
* [MetalLB](https://github.com/bitnami/charts/tree/master/bitnami/metallb)
* [Nexus](https://github.com/Oteemo/charts/tree/master/charts/sonatype-nexus)
* [Nginx Ingress](https://github.com/kubernetes/ingress-nginx/tree/master/charts/ingress-nginx)
