Configureing Kubernetes Cluster with Kubeadm
--------------------------------------------
System Requirements: 
OS: Ubuntu 20.04 LTS
Core: 2
Memory: 4 GB
Disk: 70 GB

### Change server's hostname to 'master1'
```
sudo hostnamectl set-hostname master1
bash
```

### Make sure the following line exists in /etc/hosts file.
```
127.0.1.1 master1
```

### Make sure the following lines are commented in /etc/fstab file.
```
/dev/disk/by-id/dm-uuid-LVM-3FUB4eGR4Sih9Dg3qmmXaWxQJcyGufjiqHJ8AOoU2U0FOPBXSp558oqhW69MEXVq none swap sw 0 0
/swap.img none swap sw 0 0
```

### Disable swap to prevent Kubernetes issues
```
sudo swapoff -a
### Verify swap is off
sudo swapon --show
```

### Load br_netfilter module for Kubernetes networking
```
sudo cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF
```

### Configure sysctl settings for Kubernetes networking
```
sudo cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```

### Enable IP forwarding
sudo cat <<EOF | sudo tee /proc/sys/net/ipv4/ip_forward
1
EOF

### Apply sysctl settings
```
sudo sysctl --system
```

### Download and install Docker
```
curl -fsSL https://get.docker.com -o install-docker.sh
sudo bash install-docker.sh
```

### Install Kubernetes packages and setup repository
```
sudo apt-get install -y apt-transport-https ca-certificates curl
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.27/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
sudo echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.27/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
```

### Install kubelet, kubeadm, and kubectl
```
sudo apt-get install -y kubelet kubeadm kubectl
```

### Hold the versions to prevent upgrades
```
sudo apt-mark hold kubelet kubeadm kubectl
```

### Check kubeadm version
```
kubeadm version
```

### Backup and restart containerd configuration
```
sudo mv /etc/containerd/config.toml /etc/containerd/config.toml.backup
sudo systemctl restart containerd
````

### Initialize the Kubernetes cluster
```
sudo kubeadm init
```

### Check the status of system pods
```
sudo kubectl get pods -n kube-system
```

### To start using your cluster, run the following as a regular user:
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### Alternatively, if you are the root user, you can run:
```
export KUBECONFIG=/etc/kubernetes/admin.conf
```

### Verify the status of the system pods and nodes
```
kubectl get pods -n kube-system
kubectl get nodes
```

### Install Calico network plugin for the cluster
```
curl https://raw.githubusercontent.com/projectcalico/calico/v3.24.1/manifests/calico.yaml -O
kubectl apply -f calico.yaml
```

### Wait until you get a success message from the following command
```
tail -f /var/log/syslog
```

### Verify the status of the nodes and system pods
```
kubectl get nodes
kubectl get pods -n kube-system
```
