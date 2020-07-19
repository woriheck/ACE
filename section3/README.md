#### 3.1 Deploying and implementing Compute Engine resources. Tasks include:
Launching a compute instance using Cloud Console and Cloud SDK (gcloud)
(e.g., assign disks, availability policy, SSH keys)


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
