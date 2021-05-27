# Code Engine Flask App

## Set up your environment

See the [Scalable APIs](https://github.com/Call-for-Code/Code-Engine-Scalable-APIs) example.

```bash
# Log in with your standard account, can't be a Lite account
ibmcloud login
export DOCKERHUB_USERNAME=[YOUR DOCKERHUB USERNAME]
export DOCKERHUB_PASSWORD=[YOUR DOCKERHUB PASSWORD]

# Create a resource group for your projects, or reuse an existing one
ibmcloud resource group-create call-for-code
ibmcloud target -g call-for-code

# Create a project for your applications, and a registry entry for the place to store images
ibmcloud ce project create --name flask-app
ibmcloud ce registry create \
         --name dockerhub \
         --server https://index.docker.io/v1/ \
         --username $DOCKERHUB_USERNAME \
         --password $DOCKERHUB_PASSWORD
ibmcloud ce project select --name flask-app
```

## Build and deploy

```bash
# Change to the post-cat directory
cd flask

# Build the image. Make sure the Docker daemon is running.
docker build --no-cache -t $DOCKERHUB_USERNAME/flask-app .

# And push it
docker push $DOCKERHUB_USERNAME/flask-app

# Create the app
ibmcloud ce application create --name flask-app --image $DOCKERHUB_USERNAME/flask-app

# Get app details, if needed
ibmcloud ce application get --name flask-app

# Get the URL of the app for later use
URL=$(ibmcloud ce application get --name flask-app --output url)
```

## Test

```bash
curl $URL
```
