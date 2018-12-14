# k8s-1.12.1
# Kubernetes v. 1.12.1 - fix for kubeadm join configmap get permission error

# Perform steps 1, 2, 3, 4 on the Master node.
# Perform step 5 on the Worker node.

STEP 1: Create a new "kubelet-config-1.12" ConfigMap from existing "kubelet-config-1.13" ConfigMap

STEP 2: Get token prefix

STEP 3: Create a new "kubeadm:kubelet-config-1.12" role from existing "kubeadm:kubelet-config-1.13" role

STEP 4: Create a new rolebinding "kubeadm:kubelet-config-1.12" from existing "kubeadm:kubelet-config-1.13" rolebinding

STEP 5: Run kubeadm join from Worker node
