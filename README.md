Steps to deploy:
1) Setting up environment:
   ssh-keygen -f <publickeyname>
   alias tf=terraform
   tf init
   tf validate
   tf plan
   tf apply --auto-approve

2) Copying files
   scp -i <publickeyname> <yaml file name> <ec2-publicIP>:/tmp

3) Logging in and setting up kind & kubectl:
   ssh -i <publickeyname> <ec2-publicIP>
   chmod 777 init_kind.sh
   ./init_kind.sh
   alias k=kubectl
   k get nodes
   k create ns sqldb
   
4) Running Mysql component:
   k apply -f pod-mysql.yaml -n sqldb
   k apply -f deploy-mysql.yaml -n sqldb
   k apply -f replica-mysql.yaml -n slqdb
   k apply -f service-mysql.yaml -n sqldb
   
5) Running python application component:
   k create ns webapp 
   k apply -f pod-python.yaml -n webapp
   k apply -f replica-python.yaml -n webapp
   k apply -f deploy-python.yaml -n webapp
   k apply -f deploy-service.yaml -n webapp
   k port-forward svc/python-app 8080:8080 -n webapp
   
6) Updating image version:
   k edit deployment.apps/python-app -n webapp
   change version to v2 and save file
   k apply -f deploy-pythonv2.yaml -n webapp
   k rollout history deployment.apps/python-app -n webapp
   k get pods -n webapp 
   hhf
   
