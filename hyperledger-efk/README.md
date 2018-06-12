# hyperledger-efk

This documentation describes how to deploy the IBM Blockchain Platform (Hyperledger Fabric 1.0.x + Composer 0.16.x) on minikube alongside a Elasticsearch + fluentd + Kibana stack.

## Prerequisites:

Download [OVA] (https://s3-us-west-2.amazonaws.com/dispatch-imgs/Ubuntu_16.04.2_server_amd64_dispatch.ova) and deploy it on Virtualbox or VMware. 

After booting up the OVA, change the hostname to your liking by editing /etc/hostname and /etc/hosts with your favorite editor.

## Installation

Install updates, make, nodejs + composer-cli:
```
sudo apt update && sudo apt upgrade
sudo apt install make
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo chown -R root /usr/lib/node_modules
sudo npm install -g composer-cli
```

Start Minikube:
```
./install_minikube
sudo chown -R $USER $HOME/.kube
sudo chgrp -R $USER $HOME/.kube
sudo chown -R $USER $HOME/.minikube
sudo chgrp -R $USER $HOME/.minikube
sudo minikube start
```

Create IBM Blockchain Platform
```
git clone https://github.com/IBM-Blockchain/ibm-container-service
cd ibm-container-service/cs-offerings/scripts
./create_all.sh
kubectl get svc //check the ports
cd ../../..
```

Set up EFK cluster:
```
git clone https://github.com/inovex/kubernetes-logging
cd kubernetes-logging
cd fluentd_image
sudo make build
cd ../es-image
sudo make build
cd ../kibana-image
sudo make build
cd ..
./run_demo.sh
```

To get the kibana url, type
```
minikube service -n kube-system --url kibana-logging
```
