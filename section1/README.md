#### 1.1 Setting up cloud projects and accounts
Creating projects

```
$ gcloud projects create $PROJECT_ID --name="ace-project"
```

Assigning users to predefined IAM roles within a project
```
$ gcloud iam roles list --filter="name~logging"
$ gcloud projects add-iam-policy-binding cloud-engineer-14991 --member="user:$GUSER" --role="roles/logging.viewer"
```

Managing users in Cloud Identity (manually and automated)

Enabling APIs within projects
```
$ gcloud services list --available --filter="name~stackdriver"
$ gcloud services enable stackdriver.googleapis.com
```

Provisioning one or more Stackdriver workspaces

https://cloud.google.com/monitoring/workspaces/create


#### 1.2 Managing billing configuration
Creating one or more billing accounts

Linking projects to a billing account
```
$ gcloud beta billing projects link $PROJECT_ID --billing-account="$ACCOUNT_ID"
```

Establishing billing budgets and alerts

[Set budgets and budget alerts](https://cloud.google.com/billing/docs/how-to/budgets)

Setting up billing exports to estimate daily/monthly charges

[Export Cloud Billing data to BigQuery](https://cloud.google.com/billing/docs/how-to/export-data-bigquery)

#### 1.3 Installing and configuring the command line interface (CLI), specifically the Cloud SDK.
Setting the default project

```
$ gcloud config configurations create ace-project-conf
$ gcloud config set project $PROJECT_ID

$ gcloud compute zones list --filter="name~asia"
$ gcloud config set compute/zone asia-southeast1-a
$ gcloud config set compute/region asia-southeast1
```
