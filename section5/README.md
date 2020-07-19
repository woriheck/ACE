#### 5.1 Managing identity and access management (IAM). Tasks include:

Viewing IAM role assignments
```
$ gcloud projects get-iam-policy $PROJECT_ID
```

Assigning IAM roles to accounts or Google Groups
```
$ gcloud projects add-iam-policy-binding $PROJECT_ID --member="user:$GUSER" --role="roles/logging.viewer"
$ gcloud projects add-iam-policy-binding $PROJECT_ID --member="group:$GGROUP" --role="roles/logging.viewer"
```

Defining custom IAM roles
```
$ gcloud iam roles list --filter="name~logging"
$ gcloud iam roles create "logging.metricViewer" --project=$PROJECT_ID --permissions="logging.logMetrics.get,logging.logMetrics.list"
$ gcloud iam roles describe --project=$PROJECT_ID logging.metricViewer
```

#### 5.2 Managing service accounts. Tasks include:

Managing service accounts with limited privileges

https://datarunsdeep.com.au/blog/flying-beagle/security-best-practices-using-service-accounts-google-cloud-platform

Assigning a service account to VM instances
```
$ gcloud iam service-accounts create my-vm-service-account
$ SERVICE_ACCOUNT=my-vm-service-account@$PROJECT_ID.iam.gserviceaccount.com
$ gcloud compute instances create myvm-srv --service-account=$SERVICE_ACCOUNT --machine-type=f1-micro
```

Granting access to a service account in another project

https://stackoverflow.com/questions/35479025/cross-project-management-using-service-account

#### 5.3 Viewing audit logs for project and managed services.
