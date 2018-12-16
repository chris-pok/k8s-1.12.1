<b> k8s-1.12.1 </b>

<b> Kubernetes v. 1.12.1 - fix for kubeadm join configmap get permission error </b>

<b> Perform STEPS 1, 2, 3, 4 on the Master node. </b>

<b> Perform STEP 5 on the Worker node. </b>

<b> STEP 1: Create a new "kubelet-config-1.12" ConfigMap from existing "kubelet-config-1.13" ConfigMap: </b>

    $ kubectl get cm --all-namespaces
    $ kubectl -n kube-system get cm kubelet-config-1.13 -o yaml --export > kubelet-config-1.12-cm.yaml
    $ vim kubelet-config-1.12-cm.yaml     #modify at the bottom:
                                          #name: kubelet-config-1.12
                                          #delete selfLink
    $ kubectl -n kube-system create -f kubelet-config-1.12-cm.yaml

<b> STEP 2: Get token prefix: </b>

    $ sudo kubeadm token list     #if no output, then create a token:
    $ sudo kubeadm token create
    TOKEN                       ...		...
    a0b1c2.svn4my9ifft4zxgg     ...		...
    # Token prefix is "a0b1c2"
    
<b> STEP 3: Create a new "kubeadm:kubelet-config-1.12" role from existing "kubeadm:kubelet-config-1.13" role: </b>

    $ kubectl get roles --all-namespaces
    $ kubectl -n kube-system get role kubeadm:kubelet-config-1.13 > kubeadm:kubelet-config-1.12-role.yaml
    $ vim kubeadm\:kubelet-config-1.12-role.yaml    #modify the following:
                                                    #name: kubeadm:kubelet-config-1.12
                                                    #resourceNames: kubelet-config-1.12
                                                    #delete creationTimestamp, resourceVersion, selfLink, uid (because --export option is not supported)	
    $ kubectl -n kube-system create -f kubeadm\:kubelet-config-1.12-role.yaml

<b> STEP 4: Create a new rolebinding "kubeadm:kubelet-config-1.12" from existing "kubeadm:kubelet-config-1.13" rolebinding: </b>

    $ kubectl get rolebindings --all-namespaces
    $ kubectl -n kube-system get rolebinding kubeadm:kubelet-config-1.13 > kubeadm:kubelet-config-1.12-rolebinding.yaml
    $ vim kubeadm\:kubelet-config-1.12-rolebinding.yaml   #modify the following:
                                                          #metadata/name: kubeadm:kubelet-config-1.12
                                                          #roleRef/name: kubeadm:kubelet-config-1.12
                                                          #delete creationTimestamp, resourceVersion, selfLink, uid (because --export option is not supported)
    - apiGroup: rbac.authorization.k8s.io                 #add these 3 lines as another group in "subjects:" at the bottom, with the 6 character token prefix from STEP 2
      kind: Group
      name: system:bootstrap:a0b1c2	
    $ kubectl -n kube-system create -f kubeadm\:kubelet-config-1.12-rolebinding.yaml

<b> STEP 5: Run kubeadm join from Worker node: </b>

    $ sudo kubeadm join --token <token> <master-IP>:6443 --discovery-token-ca-cert-hash sha256:<key-value> 
    # If you receive 2 ERRORS, run kubeadm join again with the following options:
    $ sudo kubeadm join --token <token> <master-IP>:6443 --discovery-token-ca-cert-hash sha256:<key-value> --ignore-preflight-errors=FileAvailable--etc-kubernetes-bootstrap-kubelet.conf,FileAvailable--etc-kubernetes-pki-ca.crt
