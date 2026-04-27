Microk8s ile kubernetes cluster kurma

İlk Node da
sudo snap install kubectl --classic --channel=1.31/stable
sudo snap install microk8s --classic --channel=1.31/stable

microk8s add-node

microk8s config > ~/.kube/config

kubectl get nodes

From the node you wish to join to this cluster, run the following:
microk8s join 10.0.0.43:25000/6e33bd9e8e224dcf8d25387f753103a5/06b5c62cc2fe

Use the '--worker' flag to join a node as a worker not running the control plane, eg:
microk8s join 10.0.0.43:25000/6e33bd9e8e224dcf8d25387f753103a5/06b5c62cc2fe --worker

If the node you are adding is not reachable through the default interface you can use one of the following:
microk8s join 10.6.8.43:25000/6e33bd9e8e224dcf8d25387f753103a5/06b5c62cc2fe
microk8s join 10.0.0.43:25000/6e33bd9e8e224dcf8d25387f753103a5/06b5c62cc2fe
microk8s join 172.16.11.43:25000/6e33bd9e8e224dcf8d25387f753103a5/06b5c62cc2fe
microk8s join 192.168.64.1:25000/6e33bd9e8e224dcf8d25387f753103a5/06b5c62cc2fe
microk8s join 172.17.0.1:25000/6e33bd9e8e224dcf8d25387f753103a5/06b5c62cc2fe
microk8s join 172.31.0.1:25000/6e33bd9e8e224dcf8d25387f753103a5/06b5c62cc2fe
microk8s join 192.168.0.1:25000/6e33bd9e8e224dcf8d25387f753103a5/06b5c62cc2fe

Diğer nodlarda
sudo snap install kubectl --classic --channel=1.31/stable
sudo snap install microk8s --classic --channel=1.31/stable

microk8s join 10.0.0.43:25000/6e33bd9e8e224dcf8d25387f753103a5/06b5c62cc2fe

microk8s config > ~/.kube/config

kubectl get nodes
