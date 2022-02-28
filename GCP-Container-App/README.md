# GCP-Container-App

This pipeline template allows you to deploy a containerized application on the Google Cloud Services.

![gcp-container-app](https://i.imgur.com/k9tsuTg.png)

* **Enable Build Service**: Enables a specified service in the Google Cloud project.
In this example, the name of the service to be enabled is ```cloudbuild.googleapis.com```.
* **Enable Cloud Run Service**: Enables the ```run.googleapis.com``` service in the Google Cloud project.
* **Enable Artifact Registry**: Enables the ```artifactregistry.googleapis.com``` service in the Google Cloud project.
* **Initialize Google Cloud**: Executes the following command using the CMD Plugin to initialize Google Cloud. 

```echo '${JSON.stringify(kaholo.execution.configuration.svcAccKeyFile)}' > /keyfile.json && gcloud auth activate-service-account --project=${kaholo.execution.configuration.project} --key-file=/keyfile.json```

* **Deploy Nginx**: Executes the following command using the CMD Plugin to deploy Nginx.

```gcloud run deploy nginx-service --image=gcr.io/google-containers/nginx --project=${kaholo.execution.configuration.project} --region=${kaholo.execution.configuration.region} --port=80 --cpu=1 --allow-unauthenticated```

* **Clone Python Repo**: Executes the following command using the CMD Plugin to clone the Python repository.

```rm -rf python-docs-samples && git clone https://github.com/GoogleCloudPlatform/python-docs-samples```

* **Deploy Python Server**: Executes the following command using the CMD Plugin to deploy the Python server.

```gcloud run deploy python-service --project=${kaholo.execution.configuration.project} --region=${kaholo.execution.configuration.region} --source=${kaholo.execution.configuration.sourceDir} --cpu=1 --allow-unauthenticated --quiet```

## Configuration variables

```
{
    "project": "cumulo",
    "region": "us-east1",
    "sourceDir": "python-docs-samples/run/helloworld",
    "svcAccKeyFile": {
        "type": "service_account",
        "project_id": "cumulo",
        "private_key_id": "b26bb38e253ca2d98e16c3315ba9a535904cf79a",
        "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvAIBADANBgkqhkiG9w0BAQEFAASCBKYwggSiAgEAAoIBAQC0MRF0ckKodp6s\n/xxvaMH5qgJns9GdkJ7weAKV6CuXMfTwh8cHeQ+JAKl6XSRFSUz3pF82Ctf+pEdL\najkOyvAH45+sIlRMVDzblZw2OQNyJtWuCBcV1xaxgJa6riLCXDP5T6k6aAeWhj0C\nT0XoLpkwuce73aAiFJlR1SOyJrx7iNXYMGc3kV+5V0Zpz8/re0lp5UX8EIoglpQa\n6ABLosmKQxTWW9ha4S1O+yVbnob1E6JscNMPqxkEmt3WK9umIAVZkzyjnXsQDtic\nUQY7qWk2gO/6Rck0Yr9jo/A8X/9YkQ3NhWGvZF+2Quhb8QeFf0g0p0ivMF97S47X\nLjaarfvNAgMBAAECggEAAy1QTPjMwxKgVpWC51X2AZRlSXXKrtgDbWL4TLNUvWw0\nkN8b+74/L1+zHUSyJISX1k/wG9BSIZu85BAB5Ihgdgsl4A+U4+miTw7Su8QCkwsr\nPk/HNGvf6w5E2hLC6yFcCzvD1JNL+vPv9G/4YBEeQ1UrN90++wrY7y/uWrBDJ3xp\nEYX8zXb9WemM2wRo2qtubSSaWvGg9dpSOl4vVIwiv2GQSZOVcANNFvAYBBngQGYR\n/LlErcl0Cvl/VOvIxMDiCKZXXs5UjMTKegvDzaqIqAL8hl003XlWA9tLe0wlmVMe\nHCfYoh/pnqHqj+YCrYPlxQE6T8eNpMqzMuuBddyX0wKBgQDbU83VNkDjzoub4IN9\n4t5zQP+XE8c9TAwusPnxU1ooUF0pUYy44JBMybpLB82nbrdgDBO4GVPz1pPvHPyU\nWWdsxWYiitJ5ZLqVRr1+RArqeV4XUwSGSjwDlqacXUBm0CGFTGbVAw2ngL6Q1wXi\n9mjZir5SwZ1aBjTb+icH0bJ3RwKBgQDSUhS7/89AOz1fTjJRIUCKQ0FLT4ghaSM/\nQ/Qbb5hu0ewyYipJjPvj5s4Zwg9+UJA84iKG9Inq7ah8FvwrYEtrYCPK3EQBF+xp\nssuBcmS1yKADcggpZYJ1FXptjQLRQX3yyZNaRvhJF5mRRhfg+xN9MmKFYSRf4jx5\nkP3LHgumSwKBgCaKJ1Ub33MoTCfr368zOluORFtLwmrm/g0GVCUvvFvxIy2rgsrJ\nyxFzMSUWDfEp51cdSCnFaZcjUYNm2ItI2n+mgDf5pNpn9NFeSNXzJufkw7/deqIO\nUVVgF47KZBKs5/cAYeF0U+XnHZVd807adbokQyVPmFLFXGx7HHinRHDpAoGAF4OD\nu+0Cn7y/xMr6RyW/kHWqdCAFKS8W+LjBLtwQH7uqe4uMDMMNWlJwkmKm1sltBtGF\naK8oSDKf1pe/Q541cGDEP6bWl4S0MrEBnYxLhCNU+G2kSfSGXE61bFAKk5iN2zn8\nFmi+03Um/x3mB4oqiKG8cAsHRQ0HE9RI+491X/cCgYAQYtmYQD5Q6D2cff8TU5g9\ncoR6Jdy2VaETZX+kK83u47HIhlbGznGELJkTXOdPhcXE4yYsyHHMdP8AqDAHaimO\nKZfNC1GnSvFH2sTIn0OK24AIdhHotG00/a3ScDgYIZYcLhqGncISUarJBRcZZgQs\ngpzGdNq+0kmVcgJ9MiNUuw==\n-----END PRIVATE KEY-----\n",
        "client_email": "owner-560@cumulo.iam.gserviceaccount.com",
        "client_id": "104848521204613996066",
        "auth_uri": "https://accounts.google.com/o/oauth2/auth",
        "token_uri": "https://oauth2.googleapis.com/token",
        "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
        "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/owner-560%40cumulo.iam.gserviceaccount.com"
    }
}
```

## Plugins used in this template

* [Google Cloud Services](https://github.com/Kaholo/kaholo-plugin-google-cloud-services) Plugin for Kaholo
* [Command Line](https://github.com/Kaholo/kaholo-plugin-cmd) Plugin for Kaholo
