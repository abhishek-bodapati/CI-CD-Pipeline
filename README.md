# Artifact Upload

* This is repo contains the Jenkins file to upload the artifact from target folder to nexus repository.
* Jenkins builds the job on every new commit.
* Jenkins would also send an email and a HTTP POST request of the build status.


## Progress
- [X] **Use Case 1**
- [X] **Use Case 2**
- [X] **Use Case 3**
- [ ] **Use Case 4** (Partially done)
- [ ] **Use Case 5**

## Use Cases

#### Use Case 1-Create Artifact Repository

- Create an artifact repository in the server
- Use Nexus/Artifactory

#### Use Case 2-Create environment specific artifact folders

- Dev/QA/Prod

#### Use Case 3-Create pipeline for uploading to artifact repository

- Should fetch the artifact from the build step and upload to the repo Should upload to an environment specific build folder (dev/QA/Prod)

#### Use Case 4-Notify portal of new artifact

- Should notify the portal if a new prod artifact is created

#### Use Case 5- Integrate with existing build pipeline

- Either use webhooks or add our pipeline script to the build pipeline yml
