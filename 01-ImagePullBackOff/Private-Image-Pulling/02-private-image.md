# Private Image

We will learn how to create a Pod that uses a Secret to pull an image from a private container image registry or repository. 

### Create a Secret by providing Docker credentials on the command line

```
kubectl create secret docker-registry demo --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
```

### Create a Secret by providing AWS ECR credentials on the command line

```
kubectl create secret docker-registry <secret_name> \
  --docker-server=${AWS_ACCOUNT}.dkr.ecr.${AWS_REGION}.amazonaws.com \
  --docker-username=AWS \
  --docker-password=$(aws ecr get-login-password --region <AWS_REGION>) \
  --namespace=default
```

### Parameters : ###

1. **`kubectl create secret docker-registry`**  
   This command creates a secret that Kubernetes uses to authenticate with container registries.

2. **`kubectl create secret docker-registry <secret_name> \
`**  
   The name of the secret. This name must match the value specified in your Kubernetes deployment's `imagePullSecrets` section.

3. **`--docker-server=${AWS_ACCOUNT}.dkr.ecr.${AWS_REGION}.amazonaws.com`**  
   - Specifies the Amazon ECR registry URL where your container images are stored.  
   - Replace `${AWS_ACCOUNT}` with your AWS account ID and `${AWS_REGION}` with the appropriate AWS region for your ECR repository.

4. **`--docker-username=AWS`**  
   - The username for Amazon ECR is always `AWS`.  
   - This is a fixed convention set by AWS.

5. **`--docker-password=$(aws ecr get-login-password --region <AWS_REGION>)`**  
   - Dynamically fetches the authentication token for your ECR repository using the AWS CLI.  
   - This token is valid for 12 hours and provides secure access to your private ECR repository.  
   - Ensure that the AWS CLI is installed, configured, and has the necessary IAM permissions to fetch the login token.

6. **`--namespace=default`**  
   - Specifies the namespace where the secret will be created.  
   - Use `default` unless