# AWS CloudFormation templates for web applications.

## Autoscaling infrastructure for a web application with basic deployment.

![Architecture diagram](media/apps-on-cloud-generic.png?raw=true "Autoscaling application architecture")

### 1- Getting started
* Usage of a new AWS account is recommended.
* Create two SSH Keys in EC2 AWS console under `Key Pairs` in the left sidebar under Networks & Security. One key for the bastion host and another for your application.
* Sample application - https://github.com/Tanbouz/cloudformation-django-sample

**These CloudFormation templates will exceed your free tier. They will cost at least 70 USD / month.**

### 2- Create the infrastructure stack
This creates a VPC with public and privates subnets. Internet gateway and a NAT gateway. An EC2 instance acting as your SSH access server or bastion host and other core resources.

* Go to AWS CloudFormation console. Make sure you are in the AWS region of your choice.
* Create a new stack
* Upload `cloud.yml` **Upload a template to Amazon S3**.
* Next
* Name your Stack. Example: `base`,`cloud` or `infrastructure`
* Select your bastion key.
* Name your project. A project can host multiple apps.
* You can leave everything else as default.
* Next to **Options** page.
* Skip Options page, Next.
* Verify Details.
* Click `Create` button to deploy.
* Once done, go to AWS EC2 console and under `Security Groups` on the left sidebar, allow inbound SSH traffic to bastion host security group to your IP address. Save.

### 3- Create the application stack
* Go to AWS CloudFormation console. Make sure you are in the AWS region of your choice.
* Create a new stack
* Upload `app.yml` **Upload a template to Amazon S3**.
* Next
* Create a GitHub **Personal access tokens** https://github.com/settings/tokens. Tick the `repo` group for a private repository or only `public_repo` for public repository. Keep this token safe (Don't commit the token to git)
* Name your Stack. Your app name for example.
* Fill in the fields or leave to default. Read description next to parameters fields for instructions.
* Select the app SSH key you created earlier.
* Next to **Options** page.
* Skip Options page, Next.
* Verify Details.
* Tick `I acknowledge that AWS CloudFormation might create IAM resources with custom names`. 
* Click `Create` button to deploy.

### 4- Usage
* Once done, the app will be available under the load balancer DNS name in AWS EC2 console "Load Balancers".
* The app will be deployed using CodePipeline and CodeDeploy.

### 5- Manual extras
* Create a SSL certificate in AWS Certificate manager and attach to the load balancer to enable HTTPS.
* Use a domain name to route traffic to your load balancer DNS name using a CNAME record or an alias record if using Route 53.
* Add IAM access to the instance role to allow access to AWS services required by the application.
* Build this stack in a different AWS account for a staging or production environment (Use AWS Organizations to manage your AWS accounts)
* Edit CodePipeline to add a build stage or manual approval stage.
* Add an e-mail or a subscriber to the deployment SNS notification topic to be notified to deployment events.
* Tweak scaling policy and autoscaling group parameters.
* Create a CloudFront deployment to optimize your web application.

### 6- Notes
* These templates do NOT create stateful services except deployment S3 bucket. Create RDS databases, cache instances and application buckets manually.
* Make sure to delete all files in the deployment S3 bucket before deleting the app CloudFormation stack.