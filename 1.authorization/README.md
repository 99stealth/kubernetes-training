## Authorization
Authorize gcloud to access the Cloud Platform with Google user credentials:
```
gcloud auth application-default login
```
Lists all properties in your active configuration:
```
gcloud config list project
```
Install kubectl tool
```
gcloud components install alpha && \
gcloud components install beta && \
gcloud components install kubectl
```