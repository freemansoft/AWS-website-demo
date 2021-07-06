**This repository was created as part of an AWS interview process**

## Prerequisites

### Create AWS Security Credentials
You need AWS security credentials in order to connect to AWS resources from your laptop. 
You need an AWS Access Key and an AWS Secret Access Key. 
Credentials can be created using the root account privs or using an IAM user account.
Root Account privs are **valid across regions**

1) Log into the console
1) Select the desired region
1) Select _My Security credentials_ in the user drop down in the upper right corner
1) You will be asked if you iwsh to _Continue to Security Credentials_ or _Get started with IAM_
    * These steps assume you are _not_ using an IAM user.
1) Select _Continue to Security Credentials_
1) Create _Accesses Keys_
1) Select _Create New Access Key_
    * This  will create an _Access Key_ and _Security Token_
1) Download the _Security Credentials_


### Create SSH Keypair 
SSH keypairs are **per account**.  This means you need different keys when you work in different regions.

#### Create Keypair using the console
Yeah, you can just create the keypair in the console but that would be lame.

#### Using CLI
1) Install the CLI
    * https://docs.aws.amazon.com/cli/latest/userguide/cli-install-macos.html
1) Set your AWS CLI defaults to set the region and your aws credentials
    * `aws configure`
    * Enter your _Access Key ID_ and _Secret Access Keys_
    * Enter your region
       * Your aws commands will hang later if you pick an invalid region here
1) Create SSH keypair.  remember the name.  The following creates a keypair called _MyKeyPair_ and saves it in a file _MyKeyPair.pem_
    * Mac `aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem`
    * Windows `aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text | out-file -encoding ascii -filepath MyKeyPair.pem`

## Deploy site

We're going to use a CloudFormation template create as a part of https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/working-with-templates-cfn-designer-walkthrough-createbasicwebserver.html#working-with-templates-cfn-designer-walkthrough-createbasicwebserver-addresources

### Deploy via console
1) Log into the console and select the correct region : 
    * us-west-2 is Oregon
1) Run cloud formation template using the console

### Deploy via AWS cli
__To Be Written__

#Random Useful Information
## SSH using .pem file
Replace the IP at the end of this command with the IP of your server:
`ssh -i MyKeyPair.pem ec2-user@34.232.53.4`

## Troubleshooting
**Problem:** Apache file access is forbidden: Every directory on the path to a file in the doc area must have x permission for all directories.
* Check permissions with `namei -m /var/www/html/index.html`
* Change permissions with `chmod 755 /var/www/html`
