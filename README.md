Upgrading kubeadm clusters:

Find the latest stable 1.20 version:
'''
yum list --showduplicates kubeadm --disableexcludes=kubernetes
'''

Upgrading control plane nodes:
'''
sudo yum install -y kubeadm-1.20.x-0 --disableexcludes=kubernetes
kubeadm version
sudo kubeadm upgrade plan
sudo kubeadm upgrade apply v1.20.x
'''

Prepare the node for maintenance by marking it unschedulable and evicting the workloads:
'''
kubectl drain k8smaster --ignore-daemonsets
'''

Upgrade kubelet and kubectl:
'''
sudo yum install -y kubelet-1.20.x-0 kubectl-1.20.x-0 --disableexcludes=kubernetes
'''

Restart the kubelet:
'''
sudo systemctl daemon-reload
sudo systemctl restart kubelet
'''

Uncordon the control plane node:
'''
kubectl uncordon k8smaster 
'''

--- Next Upgrade worker nodes: ---

Upgrade kubeadm on all worker nodes:
'''
sudo yum install -y kubeadm-1.20.x-0 --disableexcludes=kubernetes
'''

Upgrade the kubelet configuration:
'''
sudo kubeadm upgrade node
'''

Drain the node to Prepare the node for maintenance by marking it unschedulable and evicting the workloads:
'''
kubectl drain k8sworker1 --ignore-daemonsets
'''

Upgrade kubelet and kubectl:
'''
yum install -y kubelet-1.20.x-0 kubectl-1.20.x-0 --disableexcludes=kubernetes
'''

Restart the kubelet:
'''
sudo systemctl daemon-reload
sudo systemctl restart kubelet
'''

Uncordon the node 
'''
kubectl uncordon k8sworker1 
'''

Verify the status of the cluster.
'''
kubectl get nodes -o wide
kubectl version --short
'''
