# kubernetes-lab
A learning project to build Kubernetes cluster the hard way without kubeadm or other installers. This project is NOT intented for production use.

- Generates a cluster with 2 controllers and 3 nodes.
- Installs
    - Flannel
    - MetalLB
    - Helm
    - Local Provisioner (for persistant storage)
    - CoreDNS
    - Nginx-ingress
    - Prometheus
    - Grafana
    - Elasticsearch/FluentD/Kibana

## Requirements
- Vagrant
- Ansible (2.7.8 +)
- netaddr Python module

## Usage
```
sudo -H pip install netaddr ansible
vagrant up
ansible-playbook -i inventory.yml playbook.yml
```
