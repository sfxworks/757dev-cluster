# 757dev-cluster

## Joining the network as a Node Admin

### Join the Network

```bash
curl -s 'https://raw.githubusercontent.com/zerotier/ZeroTierOne/master/doc/contact%40zerotier.com.gpg' | gpg --import && \
if z=$(curl -s 'https://install.zerotier.com/' | gpg); then echo "$z" | sudo bash; fi

zerotier-cli join b6079f73c6adf9d1

ip a
```
[![asciicast](https://asciinema.org/a/3MflQKfmvkqoV2f6IsFZgiemZ.svg)](https://asciinema.org/a/3MflQKfmvkqoV2f6IsFZgiemZ)

### Submit your node for approval
https://forms.gle/2Hz3V7ZvnhSH9srD8

No hard requirements, just need to know who you are and if you're in our community. 

### Join the Cluster
[![asciicast](https://asciinema.org/a/G5TrOw0luZpakxQVeqDkp6X0v.svg)](https://asciinema.org/a/G5TrOw0luZpakxQVeqDkp6X0v)

Tested on Ubuntu 20
Set `$TOKEN` and `$NODE_IP` beforehand
```bash
modprobe br_netfilter
echo '1' > /proc/sys/net/ipv4/ip_forward
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

apt-get install -y containerd

sudo apt-get update && sudo apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF | tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm
apt-mark hold kubelet kubeadm
apt-get install ipset -y
echo "KUBELET_EXTRA_ARGS=--node-ip=$NODE_IP" > /etc/default/kubelet
echo "172.26.9.47 lab.k8s757.dev" >> /etc/hosts

#If on Rasp PI 4, enable cgroups for mem
#sed -i '$ s/$/ cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1/' /boot/firmware/cmdline.txt
#And restart

kubeadm join lab.k8s757.dev:6443 --token $TOKEN --discovery-token-ca-cert-hash sha256:01ef7baef9d250dcf764d449ac282e91666c8483bfc74994c84cc39b66301a62
```
