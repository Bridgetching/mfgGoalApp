# THE GOAL APP
- This lab teaching how to build an image using Dockerfile and pushing it to ECR.
- It a simple java app that permits you to insert your goal and it will display it.
- This app runs on port 80.
- Full ECS configuration on website.

## ECR IMAGE DEPLOYMENT:
Make sure your AWS CLI credentials are configured with the region you are working on.

## 1 - get login token- this helps for security (Optional).
- When you run the below command it will give a secure token session that will last for about 4 hours.
- Specify the correct region.
- After that run command 2.
```
aws ecr get-login-password --region us-east-1
```

## 2 - login to ecr.
- Make sure you insert your account number on the below command before running it.
```
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin {insert-aws-account-number}.dkr.ecr.us-east-1.amazonaws.com
```

## 3 - create ECR repo.
- Insert a name for your repository on the command before running it.
- Copy the ARN after craeting
```
aws ecr create-repository --repository-name {insert-repo-name} --region us-east-1 --image-scanning-configuration scanOnPush=true
```

## 4 - Build docker image from source code.
- Clone this GitHub repository to your pc and build the docker image.
- cd mfgGoalApp
- Run the first below command after inserting the name of your ECR repository making sure the dot is spaced.
- Run the second command to list the images.
```
docker build -t insert-name-of-image .
docker images
```

## 5 - NOTE: copy created repository ARN and use it for tagging.
- Here we have to tag our image before pushing to the repository.
- Use the ECR repository ARN to tag the image with.
- Run the tag command first before the push command
```
docker tag image-name give-aws-account-number.dkr.ecr.us-east-1.amazonaws.com/repo-name:latest
docker push insert-aws-account-number.dkr.ecr.us-east-1.amazonaws.com/repo-name:latest
```

## 6 - Making sure you successfully pushed the image.
- List images command below.
```
aws ecr list-images --repository-name repo-name --region us-east-1
```

## 7 - Delete image
- This command is used to delete images from ECR.
```
aws ecr batch-delete-image --repository-name {repo-name} --image-ids imageTag=latest --region us-east-1
```

## 8 - Delete Repository
- $ aws ecr delete-repository --repository-name {repo-name} --force --region us-east-1

*************************
## Pushing Code to CodeCommit

## 1 Create CodeCommit Repo.
- $ aws codecommit create-repository --repository-name {repo-name} --repository-description "CodeCommit Demo repository"

## 2 Clone CodeCommit Repo to your PC for normal AWS Admin Access.
Creating Access keys for CodeCommit:
- Open Amazon IAM => Users
- On the IAM user go to credentials =>
- On => https Git: Generate credentials => download and copy to txt file. 
- CodeCommit login will pop when you try to push to CodeCommit
- Copy CodeCommit https URL and clone on your CLI.

### FOR NORMAL IAM USERS 
- $ git clone (https URL)

## 2-B Clone CodeCommit for SSO Users.
- Make sure you have pip installed for SSO users 
- Configure your IAM credentials and Region.

#FOR SSO USERS [using HTTPS(GRC)] 
- $ pip install git-remote-codecommit 
- $ git clone codecommit::us-east-1://{repo-name} 

## 3 Push Code to CodeCommit.
You could modify the server.js file in order to reflect the difference after your pipeline runs.

- $ git add . 
- $ git status 
- $ git commit -m "message" 
- $ git push origin master 
