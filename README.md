<b> k8s-1.12.1 </b>
<b> Kubernetes v. 1.12.1 - fix for kubeadm join configmap get permission error </b>

<b> Perform STEPS 1, 2, 3, 4 on the Master node. </b>
<b> Perform STEP 5 on the Worker node. </b>

<b> STEP 1: Create a new "kubelet-config-1.12" ConfigMap from existing "kubelet-config-1.13" ConfigMap: </b>

<b> STEP 2: Get token prefix: </b>

<b> STEP 3: Create a new "kubeadm:kubelet-config-1.12" role from existing "kubeadm:kubelet-config-1.13" role: </b>

<b> STEP 4: Create a new rolebinding "kubeadm:kubelet-config-1.12" from existing "kubeadm:kubelet-config-1.13" rolebinding: </b>

<b> STEP 5: Run kubeadm join from Worker node: </b>
