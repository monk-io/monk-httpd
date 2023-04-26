# Monk & apache

This repository contains Monk.io template to deploy apache system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

## Start

Set up Monk - [https://docs.monk.io/docs/monk-in-10/](https://docs.monk.io/docs/monk-in-10/)

Start `monkd` and login.

```bash
monk login --email=<email> --password=<password>
```

## Clone Monk apache repository

In order to load templates and change configuration simply use below commands:

```bash
git clone https://github.com/monk-io/monk-apache

# and change directory to the monk-apache/apache template folder
cd monk-apache/apache
```

## Configuration

You can add/remove configuration of the template.

The current variables can be found in `stack.yaml/variables` section

```yaml
  variables:
    apache-image-tag: "latest"
```

### apache configuration file

You can find configuration file (apache.xml) in `/files` directory in repository and can edit before the running kit.

| Configuration File | Format Used | Directory in Container                 | Purpose                                                                                        |
| ------------------ | ----------- | -------------------------------------- | ---------------------------------------------------------------------------------------------- |
| **apache.xml**     | XML         | `/opt/apache/server/apache/apache.xml` | The apache.xml file defines some global configuration options that apply to all or many cores. |

## Template variables

| Variable             | Description           | Type   | Example |
| -------------------- | --------------------- | ------ | ------- |
| **apache-image-tag** | apache image version. | string | latest  |
| **http-port**        | apache image version. | int    | 8080    |
| **http-port**        | apache image version. | int    | 8443    |

## Local Deployment

| First clone the repository and simply run below command after launching `monkd`: |
| :------------------------------------------------------------------------------: |

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 apache/apache
 ├─🔗 Process groups:
 │  └─🧩 apache/stack
 └─⚙️ Entity instances:
    └─🧩 apache/apache/metadata
✔ All templates loaded successfully

➜  monk list apache

✔ Got the list
Type      Template    Repository  Version  Tags
runnable  apache/apache   local       -        self hosted, search platform,


➜  monk run apache/apache

✔ Started local/apache/apache

```

This will start the entire apache/stack.

## Cloud Deployment

To deploy the above system to your cloud provider, create a new Monk cluster and provision your instances.

```bash
➜  monk cluster new
? New cluster name apache
✔ Cluster created
Your cluster has been created successfully.

➜  monk cluster provider add -p gcp -f <path/to/your-key.json>
✔ Provider added successfully

➜  monk cluster grow -p gcp
? Cloud provider gcp
? Name of the new instance my-instance
? Tags (split by whitespace) apache
? Region europe-central2
? Zone europe-central2-a
? Instance type e2-medium
? Number of instances (or press ENTER to use default = 1) 3
? Default disk type for gcp is HDD (pd-standard). Would you like to change it? No
? Disk size (or press ENTER to use default = 250 GBs) 50
✔ Start creation of new instance(s) on gcp... DONE
✔ Creating node: my-instance-1 DONE
✔ Initializing node: my-instance-1 DONE
✔ Creating node: my-instance-2 DONE
✔ Initializing node: my-instance-2 DONE
✔ Creating node: my-instance-3 DONE
✔ Initializing node: my-instance-3 DONE
✔ Connecting: my-instance-1 DONE
✔ Syncing peer: my-instance-1 DONE
✔ Connecting: my-instance-2 DONE
✔ Connecting: my-instance-3 DONE
✔ Syncing peer: my-instance-2 DONE
✔ Syncing peer: my-instance-3 DONE
✔ Syncing nodes DONE
✔ Cluster grown successfully
```

Once cluster is ready execute the same command as for local and select your cluster (the option will appear automatically).

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 apache/apache
 └─⚙️ Entity instances:
    └─🧩 apache/apache/metadata
✔ All templates loaded successfully

➜  monk list apache

✔ Got the list
✔ Got the list
Type      Template    Repository  Version  Tags
runnable  apache/apache   local       -        self hosted, search platform,

➜  monk run apache/stack

✔ Started apache/stack

```

## Logs & Shell

```bash
# show apache logs
➜  monk logs -l 1000 -f apache/apache

# access shell in the container running apache
➜  monk shell apache/apache

```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge  --ii --rv --rs --no-confirm --rv --r  apache/apache

✔ apache/apache purged

```
