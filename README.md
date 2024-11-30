# Kubernetes Cluster Ansible Playbooks

As an example we will use single host with IP `192.168.0.101` and user `ubuntu`
To apply changes, you must run playbooks without the flag `-C` (check).

## Check that hosts are available
```bash
ansible -i environments/test all -m ping --ask-pass
```

## Prepare the system on the nodes for installing Kubernetes
```bash
ansible-playbook -i environments/test playbooks/setup-system.yml -CK --ask-pass
```

## Installing container-runtime

###  Option 1: Set `containerd` as container-runtime
On the first run, specify the variable `containerd_config_default_write=true`

```bash
ansible-playbook -i environments/test playbooks/container-runtime/containerd.yml --extra-vars containerd_config_default_write=true -CK --ask-pass
```

## Install kubernetes components
```bash
ansible-playbook -i environments/test playbooks/install-kubernetes.yml -CK --ask-pass
```

## Deploy a kubernetes cluster
```bash
ansible-playbook -i environments/test playbooks/setup-cluster.yml -CK --ask-pass
```

## Pod-networking setup
### Option 1: Use `calico` as a CNI plugin
```bash
ansible-playbook -i environments/test playbooks/pod-networking/calico.yml -CK --ask-pass
```

## Join workers

### Option 1. Cluster of one node (for test envs)
```bash
ansible-playbook -i environments/test playbooks/join-workers.yml -t single-node -CK --ask-pass
```

## [Optional] Install Helm
```bash
ansible-playbook -i environments/stage playbooks/install-helm.yml -CK --ask-pass
```

## [Optional] Installing istio
```bash
ansible-playbook -i environments/stage playbooks/install-istio.yml -CK --ask-pass
```
