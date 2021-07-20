## Error Msg
Run 

    kubeadm init --apiserver-advertise-address=192.168.1.140 --image-repository=registry.aliyuncs.com/google_containers --pod-network-cidr=10.244.0.0/16  --ignore-preflight-errors=Swap

Found

    [ERROR ImagePull]: failed to pull image registry.aliyuncs.com/google_containers/coredns:v1.8.0: output: Error response from daemon: manifest for registry.aliyuncs.com/google_containers/coredns:v1.8.0 not found: manifest unknown: manifest unknown


## What Happended
Docker images source has only coredns:1.8.0 but coredns:v1.8.0 was required. 

This might indicate a typo somewhere in some config file, but this solution will only work on how to get around this error. 

## The Plan
Pull this image manually from source, then retag this image to the required tag. 

The required tag can be found in the Error Message. 

Reset kubenetes first,
    
    kubeadm reset
    rm $HOME/.kube/config

Then,

    docker pull coredns/coredns:1.8.0
    docker tag coredns/coredns:1.8.0 registry.aliyuncs.com/google_containers/coredns:v1.8.0
    docker images

Then initialize kubeadm again,

    kubeadm init --apiserver-advertise-address=192.168.1.140 --image-repository=registry.aliyuncs.com/google_containers --pod-network-cidr=10.244.0.0/16  --ignore-preflight-errors=Swap

Deploy flannel,

    echo "151.101.128.133 raw.githubusercontent.com" >> /etc/hosts
    wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
    kubectl apply -f kube-flannel.yml

If it's a reset, **wget** can be skipped. 

Wait for a few minutes. Then check,

    kubectl get nodes -o wide
    kubectl get pods -A

You should be good to go now. 

## Other useful commands
How to remove docker images

https://stackoverflow.com/questions/32723111
https://stackoverflow.com/questions/44785585