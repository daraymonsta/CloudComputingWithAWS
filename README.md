# Cloud computing with AWS

## changed this line

## added line to check can push using SSH

## added another line

## Overview of topic/unit/week

### Big Picture
*What do we want trainees to be able to know/do by the end of this week/topic?*

*** To add ***

<br><br>
## Day 1

### Lesson structure

### Big picture
*What do we want trainees to be able to know/do by the end of this lesson?*
* Create a EC2 VM instance, SSH into it and install+run Nginx

### Needed for this lesson
* a tool for ready for drawing, e.g. jspaint.app, LucidChart
* all students need their own AWS IAM account
* (optional) private key ready so that it is ready to send to them in the chat (optional - create this file with the trainees)

### Lesson runthrough

Set expectations for repo (show a good example?).
* draw your own diagrams (DevOps Engineers need to be able to draw things out) - even put a photo of a hand drawing
* put your images in a folder (not in the repo )
* get rid of anything confidential (even the name of the key pair file)
* link your linkedin profile to your repo, repo should have a welcome page


1. T to demo:
   a. logging into to AWS
   b. change the region to Europe (Ireland) (eu-west-1)

2. T should explain:
   * no sharing of AWS credentials
   * it is enforced that you need to change your password on first login
   * credentials should not be included in your repo - you could use a password vault instead

3. T to set task (while T creates and sends the AWS logins to trainees):
   a. login to AWS and change your password
   b. create a repo called CloudComputingWithAWS (or similar)
   c. document what was was explained in the demo + what you have done so far, including what you learnt from your research yesterday about cloud computing
   d. share your repo's link

4. T to explain: 
   Advantages of ...
   * localhost is on-prem, if you something happens to your computer, you've lost everything
   * public cloud - safer (redundancy, fault tolerance, etc), also globally available
   * hybrid cloud

5. T to draw (trainees to draw their own):
* illustrate: What does a movie need before they start filming? (script) What would happen if they didn't have a script and decided to start filming an action movie? (incoherent)
* add 'laptop' icon - label it as localhost/on-prem
* add AWS cloud icon
* show passing of 'AWS credentials' to AWS cloud icon
* write list of steps (for under the diagram):
   a. migrate data - app folder, mongodb setup, we need our scripts (provision.sh), reverse proxy
   b. env var
   c. scalable - refactor monolith to 2-tier arch (explain that provision up and reloading takes a long time each time - so need a better way)
* ask: Who has bought a new laptop lately?  What did you have to consider when buying it?  (Add the answer as a new list at the bottom of the diagram): storage, cpu, mem, screen size, ssd/hdd)
* Explain that we need to know similar things for moving to cloud, then also how you will secure it e.g. with a firewall - add 'firewall - security group' to the list
* add 'cost' to the list
* add 'speed vs latency' to the list (putting the region of the cloud services close to the users)
* add another laptop icon (best icon?) - label it 'VM - vagrant'
* add an arrow from your laptop to the 'VM - Vagrant' - label the arrow 'SSH'
* Add new note at the top in a box: 'tech201.pem move to ssh folder on localhost' Explain that the key needs to match the lock 'ssh key pair'


Demo
'cd' to go to home
'pwd' - put the result in the chat
'ls -a' - instead
try to go into `.ssh` folder
'mkdir .ssh'
Explain this is where we need to copy the key (tech201.pem) to this folder

Demo
* in AWS consolte, search for EC2, show where 'key pairs' are - but tell them not to create it (we will all use the same file)
* T to send the tech201.pem via the chat
nano devops-tech201.pem
* copy & paste from downloaded file to the one in .ssh folder you made in home
* delete the one in downloads

If you need to go to London office, put into Google Maps, how many routes/options does it give you?  (2 or 3 - they will all get you there, some might be easier, others longer)

Demo - creation of EC2 instance

In running instances
Click 'Connect'
Go to 'SSH Client' tab - follow the instructions

possible problems:
if hanging, port 22 is not open
key permissions

Once ssh'ed in
do sudo apt-get update -y
do sudo apt-get upgrade -y
do sudo apt-get install nginx -y

Remember to STOP the instance when you are not using it

* Add

## Lesson 2

### Big Picture

### What's needed
Need the app + environment folder from Virtualisation lesson
Need your EC2 instance created in Lesson 1

Demo
If can't SSH in, go into the Instance, 'Security' tab, click on the link to the security group, click 'Edit inbound rules', for the port 22 rule

Demo
How then around the options for the EC2 instance

Task:
migrate app folder with provision.sh
reverse proxy
install required dependencies
cd app
npm install
npm start

Top tip (hint): rsync command or ssh with scp command to send data from one endpoint to another, you will need to include the pem file in the command

pwd for find out where pem file is
/c/Users/RamonRossi/.ssh

ssh -i "devops-tech201.pem" ubuntu@ec2-54-154-204-116.eu-west-1.compute.amazonaws.com

scp -r -P 2222 MY-FILE-SOURCE-PATH MY-FQDN@localhost:
Note: -r switch is needed to recusively copy a directory's contents

Need to copy these 2 folders:

/c/Users/ramon/'OneDrive - Sparta Global'/Documents/github/CloudComputingWithAWS/app
/c/Users/ramon/'OneDrive - Sparta Global'/Documents/github/CloudComputingWithAWS/environment

Possible problem using scp command: Connection refused message
Solution: Need to allow port 2222 in the AWS security group
Tried allowing port 2222 in security group + restarting the EC2 VM, same error (Warning)
Had to use port 22 in the scp command, then it worked.

Be careful: If you stop and start your EC2 VM instance, it's IP address will change, and you will need to modify the ssh and scp command accordingly.

These commands worked:

ssh -i "devops-tech201.pem" ubuntu@ec2-54-154-204-116.eu-west-1.compute.amazonaws.com

scp -i "devops-tech201.pem" -r -P 22 /c/Users/ramon/'OneDrive - Sparta Global'/Documents/github/CloudComputingWithAWS/app ubuntu@ec2-54-154-204-116.eu-west-1.compute.amazonaws.com:

scp -i "devops-tech201.pem" -r -P 22 /c/Users/ramon/'OneDrive - Sparta Global'/Documents/github/CloudComputingWithAWS/environment ubuntu@ec2-54-154-204-116.eu-west-1.compute.amazonaws.com:


<br><br>
## Day 2

### Big picture
Yesterday, we did the app bit, today doing the DB bit.

### Needed for lesson


### Lesson runthrough

SSH - once you have allowed the key, and allow to use it, then it is faster next time (unless you start/stop the VM, the IP will change and you will need to allow the key again the first time)

Draw first region with Ireland.

Ask: What is the benefit of having your cloud services closest to where your users are?  (speed, low latency)

Ask:
* What are the benefits of using a particular region?
* Ask who has used a grocery shop lately? Illustrate: Tesco Extra vs Tesco Express - not all the options available for every single shop - same with each datacenter - they don't all have the same options.
* How do we know which region to use?  Closest, but also need has the option you need.

Draw the rest of the diagram with App + DB.  (Make it explicit that you want the trainees to list, not take notes)

Who is making the user experience as smooth as possible? Us
User should not be able to access the DB.
The monolith is not scalable.

Give task (30min?):
Create/curate your own a 2 tier architecture diagram - answer why? when? who? how?  Publish your repo.
* Why do we need to make a 2-tier architecture ?
* Why do we need to refactor a monolithic architecture into a 2-tier?
* Why did we do it on the cloud?
* How does this fit into DevOps?## How does this fit into agile/SCRUM?

Coming back together:
Feedback on a few repos.
Let them know, you want them to interview each other this afternoon on these questions.

Set next task:
pre-requisite - app tier deployed - available on public IP on port 3000 at least
1. create 2nd tier (db) with required dependencie3s: Ubuntu 18.04LTS, install Mongodb, change configuration of mongod.conf for BindIp 0.0.0.0
2. create security group for DB - allow 27017 from anywhere - allow only from app instance
3. create env var in app instance (sets the db endpoint)
4. relaunch the app

Then draw it into a diagram - show the security rules for app, and db.

How many passed the test on a manual car?  If you pass the test, are you allowed to drive an automatic?  What about the other way around?

Steps
1. Create a EC2 instance - in AWS you need to 'launch an instance'
2. Check you can SSH into db
ssh -i "devops-tech201.pem" ubuntu@ec2-52-215-43-88.eu-west-1.compute.amazonaws.com
3. Copy the environment folder (or just provision.sh for db)
scp -i "devops-tech201.pem" -r -P 22 /c/Users/RamonRossi/'OneDrive - Sparta Global'/Documents/github/tech201_virtualisation/double-vm/environment ubuntu@ec2-52-215-43-88.eu-west-1.compute.amazonaws.com:

Alternative (will work only if the VM has access to the Internet; if it was in production environment it won't have access to the Internet): You could copy/clone this repo from:
https://github.com/khanmaster/sre_jenkins_cicd

4. make sure there is execute permissions on the provision.sh
5. run your script

ssh -i "devops-tech201.pem" ubuntu@ec2-176-34-74-205.eu-west-1.compute.amazonaws.com
scp -i "devops-tech201.pem" -r -P 22 /c/Users/RamonRossi/'OneDrive - Sparta Global'/Documents/github/tech201_virtualisation/double-vm/environment ubuntu@ec2-176-34-74-205.eu-west-1.compute.amazonaws.com:

### Possible problem
Error with db_provision.sh
sudo: unable to execute ./dbprovision.sh: No such file or directory
Add script file's location to the path. Example: `export PATH=$PATH:$HOME/environment`, then run the command `sudo ./script.sh`

### PM session

Demo
T to show how to change mongod.conf
Ask: Where is the mongod.conf file? (/etc/mongod.conf - will only be there if install mongodb)

Use diagram to help visualise what we will do
1. `cd /etc`
2. `sudo nano mongod.conf`
3. change the BindIp line to 0.0.0.0 (from 127.0.0.1)
3. confirm it's done with `cat` command
4. get db ip address from instance details e.g. 176.34.74.205
5. SSH into app VM
6. set env var DB_HOST with the new db VM's ip
Example:
```
echo "export DB_HOST=mongodb://176.34.74.205:27017/posts" >> ~/.bashrc
source ~/.bashrc
```
7. install & run the app using the commands `npm install` and `npm start`

Explain what's coming what they will be doing soon:
Making it scalable, monitor it with SMS alerts,automate it all with jenkins

Set task: Make mock interview videos of each other in groups


## Day 3

### Big picture



### What's needed for this lesson


### Lesson structure

Explain:
* difference in cost between car driving, idling, and engine stopped (but still need to pay for the parking)
* Today we will be AMI - like a copy - but much cheaper e.g. real passport (VM instance) compared to photocopy of it (AMI)

Draw diagram to help explanation:
* Start with EC2
* when you terminate the EC2 instance - it's deleted

Task (during synchronous teching): Re-starting app VM, SSH into it and get the app running.

Assessment: Trainees to provide their IP in the chat once it's working.

Potential problems when launching the app:

Problem: You do `npm start` or `node app.js` and App won't stay running: it eventually gets an error.  Reason: If the last time you run the app instance it was connected to the VM database instance, it will try to connect to the DB instance and won't be able to so will eventually give up.  Solution: Go to the app folder, and run `npm install` (without any DB_HOST env variable set), and then `npm start`.

Problem: You have started your DB instance, but the posts page won't come up.
Possible reason 1: The database instance's IP has changed.
Solution: Change the database IP used in the environment variable to setup the app's connection to the database, then do `npm install` and `npm start`.
Possible reason 2: The database has not been started+enabled on the database instance.

Demo (trainees to do it at the same time?)
1. Create an AMI of your EC2 app instance

Create the AMI instance from the 'Instance summary'
a. Click on 'action' menu
b. Click on 'Image and templates' on the menu
c. Click on 'Create image' on the submenu

2. Terminate the running EC2 app instance

Task: Finish documentation (for 15min after break)

3. Launch EC2 app instance from the AMI (created in Step 1)

a. Give it sensible name e.g. ramon-tech201-db-from-ami
b. Choose the same key pair as before
c. Select an existing security group - the one used by the same EC2 instance before (search by your name to find it easily)

Potential problems:

If you can't SSH in, it may be because your IP address has changed, and you will need to update your security group.

When you create a VM from the AMI, the default SSH login uses the `root` user.  You can't try using it, but it will give you an error about how you shouldn't use root to SSH in.  Solution: Change `root` to `ubuntu`.

T to set task:
Pre-requisite: T to 'terminate' (destroy) the EC2 database instance for all trainees
1. Re-create your EC2 database instance
2. Get the the database instance working with your EC2 app instance (/posts page should work, seeded DB, etc)
3. Once it's working, make an AMI of the working EC2 database instance.
4. Terminate the old database instance.
5. Make


### PM session

T to ask: Why do we need monitoring?  Know 

Sometimes on your laptop, fan speeds up = heavy CPU 
Sometimes on the Internet, things slow down, because of heavy traffic.

T to draw diagram with
* Resources

CCTV is not going to stop the crime.  Only recording it, is not enough.  It needs to call someone, police.

What is the CPU utilisation of my app?  If it hits 60%, ring an alarm

A smoke alarm will only monitor smoke. 

I don't want to be woken up to do something about it.  I want it to fix itself - that's where 'auto scaling' comes in.  You don't want to run an 'auto scaling' group all the time, because it costs to run it.

If you have to wake up at 3am and manually run the 'auto scaling' group, your site mimght be down half an hour.  But you can automate it, so that AWS will trigger the 'auto scaling' group.

After an incident, we want to avoid it happening again.  To help us, we can adjust the data/metrics monitored (e.g. statistics, logs) - maybe something else (e.g. the number of users) also needs monitoring to avoid problems next time.

T to show next diagram which shows the AWS services that CloudWatch can monitor:

Examples:
Amazon SQS - explain that this is a queue such as if we all click to buy the same laptop at the same time, our requests will be put in a queue

Demo `Monitoring` tab in EC2 instance
* Show how to turn on 'Detailed monitoring' (Click on 'Manage detailed monitoring' to see the option) - extra charges as it does per-min monitoring

Previously in courses, the statistics/metrics/log data was sent to Granfana or Prometheus.

T to set task:
* Decide metrics to monitor
* Create a dashboard
* Then if reaches a threshold, trigger an SNS email notification (Create an alarm)

Before they start, T to demo:
* Finding CloudWatch
* Finding Amazon CloudWatch User Guide, e.g. 'Creating a CPU usage alarm'  (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_AlarmAtThresholdEC2.html)

aws-ubuntu-version-to-select.png

## Day 4

### Big picture
Setting threshold so that it will trigger, autoscaling, VPC, want a private subnet for the database


### Session on what we'll do this week

#### Introduction / Pep talk

T to explain:
* We are now moving from 3rd gear into 4th gear.
* It's important that you help me set the pace.
* At this point, it's all about independent work, finding your own answers, documenting.

#### Tasks on the trello board for this week
Tasks for 'Doing - in progress')
* S3 buckets
  a. what is it
  b. why should we use it
  c. benefits (it's dirt cheap)
  d. disaster recovery (DR)
  e. base use cases
  f. how is it highly availabile
  g. social media storage (e.g. show image on trello)
  h. CRUD (create, read, update, delete)
  (We will do a Q&A on it tomorrow)

Tasks for this week's sprint ('Week Sprint')
* Autoscaling
* Availability zones/high availability
* Load balancers + ASGs
* AWS security
* scalable deployment
* VPCs
* AWS user-data to automate provisioning

Tasks on the backlog (for be covered in the next 4 weeks):
* SSH key pair (needed before Jenkins)
* Jenkins
* CD/CD and CDE
* AWS
* Ansible
* Terraform
* Docker
* Kubernetes (K8)
* time permits - group project


#### S3 simple storage service

a. what is it

* object storage service (similar to Azure blob storage)
* a large NoSQL database, the filename is the key and the contents is the value

b. why should we use it

* never run out of room
* global availability
* robustness
* highly scalable

c. benefits

* it's dirt cheap
* pay what you use

d. disaster recovery (DR)
e. use cases

* storage for a static web site
* backup for Shahruhk it GitHub goes down (as happened a few years ago)
* client asked for help to do something a competitor is doing - Shahruhk made a video and put it on S3 so that the client could access it

f. how is it highly availabile

* automatically stored across 3 Availability Zone (AZs) - they must be kilometeres apart, but within 100km of each other.  (Each AZ is 1 or more datacenters with redundant power, networking, connectivity).
Exceptions: 
* S3 One Zone-IA storage class is stored in a single Availability Zone
* SE on Outposts will probably store data on-prem.

g. social media storage (e.g. show image on trello)
h. CRUD (create, read, update, delete)


### Lesson

#### Big Picture for the lesson
Learn to use AWS CLI as well as the console

#### Lesson runthrough

T to draw diagram while explaining:
* AWS CLI - you will need secret keys to access
* Want to SSH into EC2, but how will we access S3 from EC2?
* Need to pass AWS secret & access keys to access S3
* What are the dependencies for AWS CLI?
  a. Python 3.7 or above
  b. update + upgrade AWS config setup
  c. AWS config
  d. enter the access key
  e. enter secret keys
  f. enter which region? (must use Ireland eu-west-1)
  g. json

Need to make sure data is secure in storage + in transit:

While data is in transit (SSHing from your laptop into EC2), how is it secured?  You will notice it says end-to-end encryption.

T to set task:
* create new Ubuntu 18.04 LTS EC2 instance, use same key pair as before, allow SSH and HTTP with new Security Group
* SSH into it

Suggested break

How do we know S3 is highly available?
Let's prove it together:
1. Go to AWS console
2. Search for 'S3' - click on the first one at the top of the list
Notice what's happened to the region.  (It's changed to 'Global')

Continue task above (do it together)
1. check internet is working on your EC2 instance (run the command `sudo apt update -y`)
2. Run the command `sudo apt upgrade -y`
Now install the dependencies:
3. `sudo apt install python -y` then check the python version with `python --version`
4. You will notice it is using an old Python version, because you did not use the `python3` command.  To solve this, we can set `alias python=python3` (though it would be best to set it as a global environment)
6. `sudo apt install python3-pip` (install the package manager for Python, it also installs the pip command)
7. `sudo pip3 install awscli`
8. Test what happens if you run this command: `aws s3 ls` (to list the files in your s3 bucket)

You should get this error.  Why?  (we haven't got access to the s3 bucket yet - need to enter your AWS secret)

![Error because no S3 bucket yet](readme_images/d4-aws-cli-trying-access-s3-without-doing-aws-configure.png)

9. `aws configure`
You will be prompted to enter your
* AWS Access Key ID
* AWS Secret Access Key
* Default region name (want it to be Ireland, so enter eu-west-1)
* Default output format: json

10. Re-run the command `aws s3 ls` to see if works now.

11. `aws s3 mb s3://ramon_tech201`

S3 has a naming convention you need to follow, otherwise it won't allow us to create a name.

![Error invalid S3 bucket name](readme_images/d4-aws-cli-s3-invalid-bucket-name.png)


11. `aws s3 mb s3://ramon-tech201`

![Show empty S3 bucket on AWS console that you created using AWS CLI](readme_images/d4-aws-cli-use-aws-console-show-empty-bucket.png)


12. create a file with some contents and check the contents is there:
```
sudo touch test.txt
nano test.txt
cat test.txt
```

15. Copy the test file to the S3 bucket you created using this command: `aws s3 cp test.txt s3://ramon-tech201`

![Show AWS CLI command results when coping a test file to an S3 bucket](readme_images/d4-aws-cli-copy-file-to-s3-bucket.png)


16. In to the AWS console, go to the file in your S3 bucket, and click on the link to view the file in your browser.  What do you notice?  (You can't view it)  Why?  (You don't have permissions yet)

![Try to view test file in S3 bucket with default permissions](readme_images/d4-aws-cli-s3-bucket-file-with-default-permissions.png)


17. Go back to the AWS console, go to the Permissions tab (It should say 'Access control list (ACL)'), and click Edit

18. Check 4 checkboxes so that there is read permission for Everyone (public access) and the Authenticated users group (anyone with an AWS account), and try to click the Save Changes button at the bottom.  What do you notice?  (It won't let you save changes unless you also check 'I understand the effect of these changes on this object')

![Try to view test file in S3 bucket with default permissions](readme_images/d4-aws-cli-s3-on-aws-console-set-public-permissions-on-file.png)

19. Try to view the file's contents again in the browser.  You should now be able to see it.

#### PM Session

T to explain:
* Using S3 we can setup access to be temporary e.g. for 2 hours
* If not accessed frequently, can be move to 'infrequent access'
* If hardly ever accessed, can be moved to 'glacier' access

Examples
* If permanent employee leaves, an employer may need to store their data from x number of years, but do you need to access it every day?
* Or a business may need (as a legal requirement) to store CCTV video footage
* Or Netflix may have a new documentary come out, the access/demand in the first week is very different to a year later

Demo (to do together, after you SSH back into your s3):
1. Remove file from your VM

T to set task (during synch)
* Download the file for your s3 bucket using the AWS CLI command:

T could ask a few students to share how they did it.  There are various ways:
`aws s3 cp s3://ramon-tech201/test.txt .`
`aws s3 cp s3://ramon-tech201/test.txt test.txt`

2. 

aws s3 rb s3://ramon-tech201

d4-aws-cli-try-to-remove-bucket-with-files-in-it.png

You will get an error because the bucket is not empty.

aws s3 rm s3://ramon-tech201/test.txt

This works for a single/file object.  Otherwise, use the `--recursive` switch.

If you have a lot of files, you could use a sync command.


T to set task:
Launch new EC2 ubuntu instance :
Next:
Research the documentation on AWS/Python for python boto3 package to create and manage AWS S3 resources and complete all the following tasks
DOD:
* Setting up awscli and python Env with required dependencies
* S3 authentication setup - with aws configure on EC2
* Create S3 bucket using python-boto3
Upload data/file to S3 bucket using python-boto3
* Retrieve content/file from S3 using python-boto3
* Delete Content from S3 using python-boto3
* Delete the bucket using python-boto3.**

Links to help:
* https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3-commands.html#using-s3-commands-delete-buckets
* https://docs.aws.amazon.com/cli/latest/reference/s3/

Timings:
* task 1 hr
* document 30min
* make a video about everything you learnt today 1hr 45min

What good looks like for your video:

https://sparta-marketing.s3.eu-west-2.amazonaws.com/Ansible_server_drift_testing_example.mp4


1. install python library boto3
sudo pip3 install boto3

Link about how to automate the retrieval of the credentials for the 'aws config':
https://boto3.amazonaws.com/v1/documentation/api/latest/guide/credentials.html


### Day 5's lesson

#### Big Picture for the lesson
Scalability

#### Possible points for discussion during standup

* Did anyone find the AWS documentation regarding Boto3 and S3 difficult?  Suggestions?  (need to find clear, simple documentation that is at your level)

AWS documentation: https://docs.aws.amazon.com/code-library/latest/ug/python_3_s3_code_examples.html

* How did you know what was going on in your Python code? Because otherwise at the end you might end up with nothing - because it deletes the bucket at the end.  (if statements using the response to the command, print statement to know what the code has done at each step)


#### Lesson runthrough

T to explain:
* Autoscaling - adds computational resources based on CPU utilisation
* What happens when there is a de-tour?  (re-directs traffic to another way)  A load balancer re-directs traffic to place that is healthy.

T to draw diagram to show an Auto Scaling Group.
* Minimim Size
* Desired capacity is 2 because one has only a single point of failure
* Maximum size

Example:
* What time do we log in?  9am.  The Teams people know that it needs to scale when everyone starts using it.  So it could scale scale on a schedule.  It could also scale on demand (when it reaches a theshold).

Application Load Balancer
Elastic Load Balancer is the most available and newest.

Add to diagram:
Auto Scale Policy
* CPU 50%
* min 2
* desired 2
* max 3
* ALB (application load balancer)

Target groups HTTP (80) - any other ports required
We will create:
* Launch Template
* ALB

T to explain on diagram:
* Why should be put instance A and instance B in different availability zones?
* instance C can be in a 3rd availability zone
* instance D cannot be in a 4th, as Ireland only has 3 availability zones

T to update Trello
* move done tasks to Done


T to demo:
* Go to AWS console
* Go to Launch Templates, then click 'Create Launch Template'
* name it something like 'ramon-tech201-ASG-1', use t2.micro, Ubuntu 18.04 LTS, same key pair as before, 
* Advanced configuration, go to User data:
put in bash


We don't have a load balancer, Auto Scaling Policy

1. Go to 'Auto Scaling Groups'
2. Click button 'Create an Auto Scaling group'
3. Name it something like 'ramon-tech201-ASG-app', select the launch template you created above
4.
Screen 1: 
Screen 2:
Screen 3:
* Instance scale-in protection - not needed

Add tag:
* Key: 'Name'
* Value: ramon-tech201-HA-SC
(HA=high availability, SC=scaling)

Example: Boxing Day deals or 'Pizza day Tuesday' - you know when the traffic will increase.

In production, you would not delete the ASG, then create a new one, as that would create downtime.  Instead, you would use versioning (the old version would be deleted)

You can use a schedule policy, to bring up/down an Auto Scaling group at certain times.

What happens if you delete the Auto Scaling Group?  (the instances launched from the Launch Templates will be terminated)

Task:
Do the same as what we have done (creating an ASG), but with your app AMI.

Steps:
1. Get the provision script working for the app instance.  NEW!  
2. Create the AMI based on the app
3. Create a Auto Scale group based on the app AMI
4. Test it - does it work?

Breaking down step 1 - How to run 'npm start' in the background

Reference: https://medium.com/idomongodb/how-to-npm-run-start-at-the-background-%EF%B8%8F-64ddda7c1f1

1. Install pm2 (process manager) using the command `sudo npm install pm2 -g`
2. Run the process using pm2: `pm2 --name HelloSpartaApp start npm -- start` (you don't have to give it a name for it to work)
3. List processes running with pm2: `pm2 ps`
4. Stop the process with ID 0: `pm2 delete 0`
5. View the logs: `pm2 logs`



Tomorrow:
You will need to need to create your own private subnet.
