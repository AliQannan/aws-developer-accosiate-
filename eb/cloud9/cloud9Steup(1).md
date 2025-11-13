# Setup Cloud 9  , Create Env , Chose  resources
##  First, let's install the c9 utility, which makes it easy to open and edit files in Cloud9 from the terminal.
```sh 
npm i c9 -g
```


## install EB CLI 

```sh 
## Install EB CLI

https://github.com/aws/aws-elastic-beanstalk-cli-setup

git clone https://github.com/aws/aws-elastic-beanstalk-cli-setup.git
python ./aws-elastic-beanstalk-cli-setup/scripts/ebcli_installer.py
```

### We don't need to keep this folder around any longer since the CLI is now installed, so let's remove it.

```sh 
rm -rf ./aws-elastic-beanstalk-cli-setup

```