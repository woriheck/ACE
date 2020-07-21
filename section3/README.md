#### 3.1 Deploying and implementing Compute Engine resources. Tasks include:
Launching a compute instance using Cloud Console and Cloud SDK (gcloud)
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

Assign ssh keys
```
$ gcloud compute instances create myvm --machine-type=f1-micro
$ ssh-keygen -t rsa -C woriheck
$ echo woriheck:$(cat /home/erikchow1993/.ssh/id_rsa.pub) > ssh-list
$ gcloud compute instances add-metadata myvm --metadata-from-file ssh-keys=ssh-list
$ ssh woriheck@{my_vm_ip}
```
https://cloud.google.com/compute/docs/instances/adding-removing-ssh-keys#instance-only
https://cloud.google.com/compute/docs/instances/adding-removing-ssh-keys#instance-only

Creating an autoscaled managed instance group using an instance template
```
$ gcloud compute instance-templates create f1-micro-template --machine-type=f1-micro
$ gcloud compute instance-groups managed create f1-micro-auto-scaling-group --template=f1-micro-template --size=1
$ gcloud compute instance-groups managed set-autoscaling f1-micro-auto-scaling-group --max-num-replicas=3 --target-cpu-utilization=0.7
```

Configuring a VM for Stackdriver monitoring and logging

Assessing compute quotas and requesting increases

Check quota

`$gcloud compute project-info describe --project $PROJECT`

https://cloud.google.com/compute/quotas

Installing the Stackdriver Agent for monitoring and logging
