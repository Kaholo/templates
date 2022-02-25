# GCP-deploy-static-site

This pipeline template allows you to deploy a static website on GCP. It imports your GCP profile and runs the configuration set project. The pipeline cleans up your working directory and clones the git repository as specified. The dependencies are also installed and the build directory is created. Then, the pipeline creates a new bucket on GCP where you can easily upload files, edit permissions, and set the index.

![gcp-deploy-static-site](https://i.imgur.com/h5ViSea.png)

**Steps:**

* **Configure gsutil**: Imports your GCP profile by executing the ```import_gcp_profile()``` command.
* **Set project**: Gets your GCP Project ID by executing the ```set_project()``` command.
* **Remove and clean repository**: Cleans up the working directory containing the git repository.
* **Clone repository**: Clones the specified git repository. 
* **Install dependencies**: Executes the command ```npm install```.
* **Run build**: Executes the command ```npm run build```.
* **Create a new Bucket**: Creates a new bucket.
* **Upload files**: Executes the command ```upload_files()```.
* **Edit permissions**: Executes the command ```edit_permissions()```.
* **Set index**: Executes the command ```set_index()```.

## How to use

To use this pipeline template you will need to edit the variables from the Code tab.

Note that the GCP bucket, BUCKET_NAME, needs to be a unique name. It is suggested to use random characters at the end of the variable name.

Additionally, you will need to update the SECRET_KEY variable name to match the key name in the Kaholo Vault. The GCP Credentials is a service account created within the project ID.

Lastly, if the git repository is a private repository, then choose the method **Clone Private Repository** and include the credentials needed for authentication. The **Clone Public Repository** does not require credentials.

## Functions 

This pipeline template includes the following variables defined in the Code tab:

```
const WORKING_DIR = "/tmp";
const GIT_REPO = "";
const GIT_BRANCH = "";
const GIT_DEST = "/tmp/git_dest_example/";

// This project has to be created:

const PROJECT_ID = "kaholo";
const SECRET_KEY = "GCP Service Account (kaholo)"

// This is the folder created after you build your project.
// It is ususally called `public`, `dist`, `build`.
const BUILD_PATH = GIT_DEST + "public"

// Make sure the bucket name is unique.
const BUCKET_NAME = "frontend-jr555"

// This is the storage class of the GCP Cloud Storage bucket.
const STORAGE_CLASS = "STANDARD";

// Location of the GCP bucket.
const LOCATION = "us-central1";
```

This pipeline template includes the following functions defined in the Code tab. 

```
async function get_key() {
    return await kaholo.vault.getValueByKey(SECRET_KEY);
}

async function import_gcp_profile() {
    const KEY = await kaholo.vault.getValueByKey(SECRET_KEY);
    return `echo '${KEY}' > key.json; gcloud auth activate-service-account --key-file key.json`;
}

function set_project() {
    return `gcloud config set project ${PROJECT_ID}`
}

function upload_files() {
    return `gsutil cp -r ${BUILD_PATH}/* gs://${BUCKET_NAME}`
}

function edit_permissions() {
    return `gsutil iam ch allUsers:objectViewer gs://${BUCKET_NAME}`
}

function set_index() {
    return `gsutil web set -m index.html -e 404.html gs://${BUCKET_NAME}`
}
```

## Plugins used in this template

* [Command Line](https://github.com/Kaholo/kaholo-plugin-cmd) Plugin for Kaholo
* [git](https://github.com/Kaholo/kaholo-plugin-git) Plugin for Kaholo
* [Google Cloud Storage](https://github.com/Kaholo/kaholo-plugin-GoogleCloudStorage) Plugin for Kaholo

