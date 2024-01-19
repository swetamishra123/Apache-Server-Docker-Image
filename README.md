
# Apache Server Docker Image

This repository contains a Dockerfile to build an Apache server image and instructions on how to run it using Docker.

## Prerequisites

- An AWS account to create an EC2 instance.
- Docker installed on your local machine. You can download Docker from [https://www.docker.com/](https://www.docker.com/).
- AWS CLI installed on your local machine. You can download it from [https://aws.amazon.com/cli/](https://aws.amazon.com/cli/).

## Usage

### Step 1: Create an EC2 Instance

1. Open the AWS Management Console and navigate to the EC2 Dashboard.

2. Click on "Launch Instance" to create a new EC2 instance.

3. Follow the steps in the EC2 instance launch wizard, selecting an Amazon Machine Image (AMI), choosing an instance type, configuring instance details, adding storage, configuring security groups, and reviewing.

4. In the "Configure Security Group" step, make sure to add a rule to allow incoming traffic on port 80 (HTTP).

5. Launch the instance and download the key pair.

6. Connect to your EC2 instance using SSH:

   ```bash
   ssh -i your-key-pair.pem ec2-user@your-instance-ip
   ```

   Replace `your-key-pair.pem` with the path to your key pair file and `your-instance-ip` with your EC2 instance's public IP address.

### Step 2: Install Docker on the EC2 Instance

1. Update the package list:

   ```bash
   sudo yum update -y
   ```

2. Install Docker:

   ```bash
   sudo amazon-linux-extras install docker
   sudo service docker start
   sudo usermod -a -G docker ec2-user
   ```

   Note: You may need to re-login or run `su - ec2-user` to apply the group changes.

### Step 3: Pull the Docker Image

```bash
docker pull httpd
```

This command pulls the official Apache HTTP Server Docker image from Docker Hub.

### Step 4: Run the Apache Server Container

```bash
docker run -d --name apache -p 80:80 httpd
```

- `-d`: Run the container in the background.
- `--name apache`: Assign the name "apache" to the container.
- `-p 80:80`: Map port 80 on the host to port 80 on the container.
- `httpd`: The name of the Docker image.

### Step 5: Access the Apache Server

Open your web browser and navigate to your EC2 instance's public IP address:

```bash
http://your-instance-ip
```

For example, if your instance's IP address is `54.123.456.789`, access the Apache server by navigating to [http://54.123.456.789](http://54.123.456.789) in your web browser.

## Stop and Remove the Container

```bash
docker stop apache
docker rm apache
```

Replace `apache` with the name you assigned to your container.

## License

This project is licensed under the [MIT License](LICENSE).
