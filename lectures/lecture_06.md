# setup (remind)
* move the project directory (e.g. `cd C:\Projects\MyApps`)
* setup a python environment: `pipenv shell`
* run VSCode: `code .`

# AWS
* create an EC2 instance (t2.micro)
* create a key pair -> keep it on your local machine (ppk)
* tag -> inbound rule -> my IP
* Elastic IP -> create -> assign

# Connection to AWS server
* Download putty (putty.org)
  * alternative -> putty.exe
  * setup key pair (connection - ssh - auth - private key file for authentication)
* File transfer (optional)
  * Download winscp
  * import from putty
  * setup auth (adv menu)

# Dev env setup on ec2 server
* install git
  * sudo yum update -y
  * sudo yum install git -y
  * git version
  * git config --global user.name "YOUR NAME"
  * git config --global user.email "YOUR@EMAIL.com"
  * git clone [url]
