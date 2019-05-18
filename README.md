# kubernetes-lab
A learning project to build Kubernetes cluster the hard way without kubeadm or other installers. This project is NOT intented for production use.

- Generates a cluster with 2 controllers and 3 nodes.
- Installs:
    - [Flannel](https://github.com/coreos/flannel)
    - [MetalLB](https://github.com/danderson/metallb)
    - [Helm](https://github.com/helm/helm)
    - [Local Statis Provisioner (for persistant storage)](https://github.com/kubernetes-sigs/sig-storage-local-static-provisioner)
    - [CoreDNS](https://github.com/coredns/deployment/tree/master/kubernetes)
    - [Nginx-ingress](https://github.com/kubernetes/ingress-nginx)
    - [Prometheus](https://github.com/prometheus/prometheus)
    - [Grafana](https://github.com/grafana/grafana)
    - [Elasticsearch](https://github.com/elastic/elasticsearch)/[FluentD](https://github.com/fluent/fluentd)/[Kibana](https://github.com/elastic/kibana)

## Requirements
- [Vagrant](https://github.com/hashicorp/vagrant)
- [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest)
- [Ansible (2.7.8 +)](https://github.com/ansible/ansible)
- [netaddr Python module](https://github.com/drkjam/netaddr)

## Usage
```
pip install netaddr ansible
vagrant plugin install vagrant-vbguest
vagrant up
ansible-playbook -i inventory.yml playbook.yml
```
