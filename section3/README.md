#### 3.1 Deploying and implementing Compute Engine resources. Tasks include:

3.1.1 Launching a compute instance using Cloud Console and Cloud SDK (gcloud)
(e.g., assign disks, availability policy, SSH keys)

Create disk and assign disk
```
$ gcloud compute disk-types list --filter="name~standard"
$ gcloud compute disks create myvm-disk-10gb --size=10GB --type="pd-standard"
$ gcloud compute disks list
$ gcloud compute instances create myvm --machine-type=f1-micro --disk="name=myvm-disk-10gb"
```

Create disk on the spot of vm creation
```
$ gcloud compute instances create myvm2 --machine-type=f1-micro --create-disk="name=myvm2-disk-10gb,size=10GB,type=pd-standard"
$ gcloud compute disks list
```

Availability policy

```
--preemptible
A preemptible VM costs much less, but lasts only 24 hours. It can be terminated sooner due to system demands

--no-restart-on-failure
Compute engine will not restart, if they are terminated for non-user-initiated reasons 
(maintenance event, hardware failure, software failure and so on)

--maintenance-policy=migrate|terminate
When Compute Engine performs periodic infrastructure maintenance
migrate: It can migrate your VM instances to other hardware without downtime
terminate: Terminate the vm

$ gcloud compute instances create myvm1 --machine-type=f1-micro --preemptible --no-restart-on-failure --maintenance-policy=terminate
```

https://cloud.google.com/compute/docs/instances/adding-removing-ssh-keys#instance-only
https://cloud.google.com/compute/docs/instances/adding-removing-ssh-keys#instance-only

3.1.2 Creating an autoscaled managed instance group using an instance template
```
$ gcloud compute instance-templates create f1-micro-template --machine-type=f1-micro
$ gcloud compute instance-groups managed create f1-micro-auto-scaling-group --template=f1-micro-template --size=1
$ gcloud compute instance-groups managed set-autoscaling f1-micro-auto-scaling-group --max-num-replicas=3 --target-cpu-utilization=0.7
```

3.1.3 Generating/uploading a custom SSH key for instances
```
$ gcloud compute instances create myvm --machine-type=f1-micro
$ ssh-keygen -t rsa -C woriheck
$ echo woriheck:$(cat /home/erikchow1993/.ssh/id_rsa.pub) > ssh-list
$ gcloud compute instances add-metadata myvm --metadata-from-file ssh-keys=ssh-list
$ ssh woriheck@{my_vm_ip}
```

3.1.4 Configuring a VM for Stackdriver monitoring and logging
3.1.6 Installing the Stackdriver Agent for monitoring and logging
Create startup scripts "startup.sh"
```
#!/bin/bash

sudo apt-get -y  update
sudo apt-get -y  install apache2 php7.0


curl -sSO https://dl.google.com/cloudagents/add-monitoring-agent-repo.sh
sudo bash add-monitoring-agent-repo.sh
sudo apt-get -y update
sudo apt-get -y install stackdriver-agent
sudo service stackdriver-agent start

curl -sSO https://dl.google.com/cloudagents/add-logging-agent-repo.sh
sudo bash add-logging-agent-repo.sh
sudo apt-get -y update
sudo apt-get -y install google-fluentd
sudo apt-get -y install google-fluentd-catch-all-config-structured
```

Run vm with startup script
```
$ gcloud compute instances create myvm --machine-type=f1-micro --metadata-from-file="startup-script=startup.sh" --tags="http-server,https-server"
```


Resouces link: https://cloud.google.com/monitoring/quickstart-lamp

3.1.5 Assessing compute quotas and requesting increases

Check project quota

```
$ gcloud compute project-info describe --project $PROJECT
```

Request for quote upgrade

https://cloud.google.com/compute/quotas


#### 3.2 Deploying and implementing Google Kubernetes Engine resources. Tasks include:
3.2.1 Deploying a Google Kubernetes Engine cluster

3.2.2 Deploying a container application to Google Kubernetes Engine using pods

https://kubernetes.io/docs/reference/kubectl/cheatsheet/

https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app

```
# Create a kubernetes cluster
gcloud container clusters create hello-cluster
# Verify the instances created
gcloud compute instances list

# Create a Kubernetes Deployment for your hello-app Docker image.
$ kubectl create deployment hello-app --image=gcr.io/${PROJECT_ID}/hello-app:v1

# Set the baseline number of Deployment replicas to 3.
$ kubectl scale deployment hello-app --replicas=3

# Create a HorizontalPodAutoscaler resource for your Deployment.
$ kubectl autoscale deployment hello-app --cpu-percent=80 --min=1 --max=5

# Expose the sample app to the internet
$ kubectl expose deployment hello-app --name=hello-app-service --type=LoadBalancer --port 80 --target-port 8080


# Apply a rolling update to the existing deployment with an image update:
$ kubectl get deployments
$ kubectl set image deployment/hello-app hello-app=gcr.io/${PROJECT_ID}/hello-app:v2
```

```
# Verify status
$ kubectl get pods
$ kubectl get service
$ kubectl get deployment
```

3.2.3 Configuring Google Kubernetes Engine application monitoring and logging

