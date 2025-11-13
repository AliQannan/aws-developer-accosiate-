## First, we'll need the MAC address, which we'll get using the EC2 Metadata service
```sh
curl -s http://169.254.169.254/latest/meta-data/mac 
```

## For me, this returns 12:02:df:df:2e:6f. For you, this will be different.

```sh
curl -s http://169.254.169.254/latest/meta-data/network/interfaces/macs/<your_mac>/security-group-ids 
```
## For me, this returns a single Security Group ID sg-0a944253f24c2cd57. For you, this will be different.

### You will also need your own IP address to grant access to your physical location

```sh
curl http://checkip.amazonaws.com/ and copy your IP address. 
```

### Then 
```sh
aws ec2 authorize-security-group-ingress --group-id <sg-id> \
--port 8080 \
--protocol tcp \
--cidr <my-ip>/32 
```

### 💡 /32 represents sets the range of the CIDR Block to be 1 IP address. Go to https://cidr.xyz/ and enter in /32 to visually see.

### When you paste the command, you'll have no output. We can use the Console to verify if the change was made to the Security Group but let's use the AWS CLI again

```sh 
aws ec2 describe-security-groups --group-ids <sg-id> \
--output text \
--filters Name=ip-permission.to-port,Values=8080
```

### This should output something like this, and we should see our IP there. If nothing was returned, we did not set the Security Group Inbound rule correctly.

```sh
SECURITYGROUPS  Security group for AWS Cloud9 environment aws-cloud9-DevEnv-22dc99ce8de244aea677fc89033e948c    sg-0a944253f24c2cd57    aws-cloud9-DevEnv-22dc99ce8de244aea677fc89033e948c-InstanceSecurityGroup-1LHXTSW5GVUBQ  123456789012    vpc-74fbd80f
IPPERMISSIONS   8080    tcp     8080
IPRANGES        172.98.67.95/32
IPPERMISSIONS   22      tcp     22
IPRANGES        35.172.155.96/27
IPRANGES        35.172.155.192/27
IPPERMISSIONSEGRESS     -1
IPRANGES        0.0.0.0/0
TAGS    aws:cloudformation:stack-id     arn:aws:cloudformation:us-east-1:123456789012:stack/aws-cloud9-DevEnv-22dc99ce8de244aea677fc89033e948c/5c318980-4c26-11ea-ade5-0e8a1126433d
TAGS    aws:cloudformation:stack-name   aws-cloud9-DevEnv-22dc99ce8de244aea677fc89033e948c
TAGS    Name    aws-cloud9-DevEnv-22dc99ce8de244aea677fc89033e948c
TAGS    aws:cloudformation:logical-id   InstanceSecurityGroup
TAGS    aws:cloud9:owner        123456789012
TAGS    aws:cloud9:environment  22dc99ce8de244aea677fc89033e948c 
```

### Now we can begin to preview our application.

### We are going to need the public IP address of the current Cloud9 environment. We could navigate the AWS Console to EC2 and get the public IP address but let us make use of the Metadata service:
```sh
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```

### This returned to me 54.234.44.97 for you it will be different. You'll need to hold onto this value.

###    ⚠️ Port 8080 is the default port for Cloud9, for production, we'll need to use Port 80

```sh
PORT=8080; npm start 
```

## Finally, in another new web browser window, open up:

```sh
 http://<cloud9-ip>:8080
```