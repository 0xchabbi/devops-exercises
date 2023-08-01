## Certified Kubernetes Administrator (CKA)

### Pods

<details>
<summary>Deploy a pod called web-1985 using the nginx:alpine image</code></summary><br><b>

`kubectl run web-1985 --image=nginx:alpine --restart=Never`
</b></details>

<details>
<summary>How to find out on which node a certain pod is running?</summary><br><b>

`kubectl get po -o wide`
</b></details>


<details>
<summary>Upgrade the current version of kubernetes from 1.25.0 to 1.26.0 exactly using the kubeadm utility. Make sure that the upgrade is carried out one node at a time starting with the controlplane node. To minimize downtime, the deployment gold-nginx should be rescheduled on an alternate node before upgrading each node.
Upgrade controlplane node first and drain node node01 before upgrading it. Pods for gold-nginx should run on the controlplane node subsequently.
</summary><br><b>

`root@controlplane:~# kubectl drain controlplane --ignore-daemonsets
root@controlplane:~# apt update
root@controlplane:~# apt-get install kubeadm=1.26.0-00
root@controlplane:~# kubeadm upgrade plan v1.26.0
root@controlplane:~# kubeadm upgrade apply v1.26.0
root@controlplane:~# apt-get install kubelet=1.26.0-00
root@controlplane:~# systemctl daemon-reload
root@controlplane:~# systemctl restart kubelet
root@controlplane:~# kubectl uncordon controlplane
then:
# Identify the taint first. 
root@controlplane:~# kubectl describe node controlplane | grep -i taint
# Remove the taint with help of "kubectl taint" command.
root@controlplane:~# kubectl taint node controlplane node-role.kubernetes.io/control-plane:NoSchedule-
# Verify it, the taint has been removed successfully.  
root@controlplane:~# kubectl describe node controlplane | grep -i taint
then:
root@controlplane:~# kubectl drain node01 --ignore-daemonsets
dann: 
root@node01:~# apt update
root@node01:~# apt-get install kubeadm=1.26.0-00
root@node01:~# kubeadm upgrade node
root@node01:~# apt-get install kubelet=1.26.0-00
root@node01:~# systemctl daemon-reload
root@node01:~# systemctl restart kubelet
then:
root@controlplane:~# kubectl uncordon node01
root@controlplane:~# kubectl get pods -o wide | grep gold (make sure this is scheduled on node)
`
</b></details>
