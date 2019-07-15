# Minio-OpenShift

This template deploys a private object store using [Minio](https://min.io/) in [Rahti](https://rahti.csc.fi/), CSC's OpenShift cluster. The template creates a single pod deployment for Minio, which uses PVC as backend volume for storing data. PVC size is provided to template as parameters & existing PVC could be also used.

##Usage

### Using Rahti Web Interface
1. Create a new project with the "Create Project" button.
  * Add project description as your CSC project for ex. "csc_project: project_100123"
2. Select the new project by clicking its name under "My Projects".
3. Click the "Import YAML/JSON" button.
4. Upload minio.yaml or copy & paste its content.
5. Click Create, You can optionally check the checkbox "Save Template" which will make this template available for others with in your Rahti project.
6. Continue & provide parameters values, finally click create.
7. Once deployment is successful, Click on "Application > Routes" to get URL of your deployed Minio object store.
8. Login to your Minio object store using Access & Secret Key.

### Using OC Command Line tool
1. Download & Install OC CLI tool. [Instructions.](https://docs.okd.io/latest/cli_reference/get_started_cli.html)
2. Login to Rahti using the `oc login` command. You can find
   instructions in the [Rahti documentation](https://rahti.csc.fi/usage/cli/):

   ```bash
   oc login https://rahti.csc.fi:8443 --token=<hidden token from Rahti>
   ```
3. To begin with, you need to create your Rahti project, you could do so by using oc OC tool
 ```bash
oc new-project <your Rahti project name> --description="csc_project: <your CSC project name>"
for example:
oc new-project demo --description="csc_project: project_100123"
```
4. Deploy your python application from GitHub using [S2I utility](https://docs.openshift.com/container-platform/3.6/creating_images/s2i.html). With OpenShift's S2I utility, you can directly deploy your applications in Rahti from application source code hosted in some VCS for ex. GitHub.
```bash
# In this example we use current repository to deploy a demo Python Flask web application.
oc new-app https://github.com/shukapoo/rahti-demo.git --name web-demo -e APP_FILE=src/app.py
```
## Parameters to be supplied
|Parameter|	Description|
|---------|------------|
|Access Key	| Access key for your Minio object store, its length should be between minimum 3 characters.|
|Secret Key	|Secret key for your Minio Object store , its length should be between 8 & 40 characters.|
|Cluster Name	|Name of the minio cluster instance which must be DNS label compatible|
|Domain Suffix	| Hostname suffix of the application.|
|PVC Name |	PVC name to mount for minio buckets|
|Storage Size|	Your Minio Object store's backend volume size|
|Whitelist|	IP whitelist for the application. Must not contain errors, otherwise Rahti will allow all traffic.|
