# Minio-OpenShift

This template for deploying a private S3 API supporting object store [Minio](https://min.io/) in [Rahti](https://rahti.csc.fi/), CSC's Kubernetes cluster. The template creates a single pod deployment for Minio from [minio/minio](https://hub.docker.com/r/minio/minio) public container image. The application uses a persistent volume as a backend data storage. The volume size is provided as a template parameter for a newly created persistent volume, but if an existing volume of the given name exists a new one will not be created and Rahti will issue an error but the Minio instance will still utilize the existing volume. Please follow [Minio User Guide](https://docs.min.io/docs/minio-quickstart-guide.htmlsyntax/) for usage of Minio object store.

## Usage

### Using Rahti Web Interface
1. Create a new project with the "Create Project" button.
  * Add project description as your CSC project for ex. "csc_project: project_100123"
2. Select the new project by clicking its name under "My Projects".
3. Click the "Import YAML/JSON" button.
4. Upload minio.yaml or copy & paste its content.
5. Click Create, You can optionally check the checkbox "Save Template" which will make this template available for others with in your Rahti project.
6. Continue & provide [parameters](#parameters-to-be-supplied) values, finally click create.
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
4. Deploy your Minio object store from deployment template, remember to supply value of [parameters](#parameters-to-be-supplied) for deployment.  
```bash
oc new-app -f <path to your template> -p PARAM1=Value1 -p PARAM2=VALUE2 -p PARAM3=VALUE3
for example:
oc new-app -f Minio-OpenShift/minio.yaml -p CLUSTER_NAME=skminio -p STORAGE_SIZE=2Gi
```
## Parameters to be supplied

|Parameter|	Description|
|---------|------------|
|Access Key	| Access key for your Minio object store, its length should be between minimum 3 characters.|
|Secret Key	|Secret key for your Minio Object store , its length should be between 8 & 40 characters.|
|Cluster Name	|Name of the Minio cluster instance, this name must be DNS compatible name|
|Domain Suffix	| Hostname suffix of the application.|
|PVC Name |	PVC name to mount for your Minio buckets. In case you want to use existing volume in your project, please provide its name|
|Storage Size|	Your Minio Object store's backend volume size|
|Whitelist|	IP address block (CIDR) from which traffic is allowed to your Minio Object Store. In case left blank or errors in IP address block, Rahti will allow all traffic from internet |
