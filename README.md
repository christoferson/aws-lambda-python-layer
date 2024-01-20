# aws-lambda-python-layer
AWS Lambda Python Layer

Step-1: Provision EC2
Provision the server to build and package using infra/ec2-builder.yaml cloudformation template

Step-2: Login to the EC2
Use SSM to login to EC2

Step-3: Install Docker
sudo yum update -y
sudo amazon-linux-extras install -y docker
amazon-linux-extras | grep docker
sudo systemctl start docker
sudo chmod 666 /var/run/docker.sock

Step-4: Pull the image
https://gallery.ecr.aws/sam/build-python3.11
docker pull public.ecr.aws/sam/build-python3.11:1.107.0-20240110200317

Step-5: Install all required packages
docker run -it -v $(pwd):/var/task public.ecr.aws/sam/build-python3.11:1.107.0-20240110200317
pip install pandas -t ./python --upgrade
pip install beautifulsoup4 -t ./python --upgrade
pip install lxml -t ./python --upgrade
pip install chardet -t ./python --upgrade

Step-6: Zip the layer package
zip -r python-3-11.zip ./python

Step 7: Upload the artifact to S3
aws s3 cp python-3-11.zip s3://<bucket>/lambda/layer/python-3-11.zip