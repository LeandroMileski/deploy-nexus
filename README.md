# deploy-nexus

Goal: Have a running Nexus Repository Manager instance accessible on port 8081.

<img width="451" height="57" alt="image" src="https://github.com/user-attachments/assets/a24d7b51-4042-4ec5-b17d-ab56583ec392" />


Task 1 — Provision an EC2 instance
Spin up a server that will host Nexus.

Use the AWS console or boto3/Terraform
Recommended: t3.medium or larger (Nexus needs at least 2GB RAM)
AMI: Amazon Linux 2023
Open inbound ports: 22 (SSH) and 8081 (Nexus UI)
Tag it so you can find it: Name: nexus-server


Task 2 — Install Java on the server
Nexus runs on the JVM so Java must be present first.

Nexus 3.x requires Java 17
Install via: dnf install java-17-amazon-corretto -y

Task 3 — Download and install Nexus
Fetch the Nexus tarball and extract it to the right place.

Download from Sonatype: https://download.sonatype.com/nexus/3/latest-unix.tar.gz
sudo wget https://download.sonatype.com/nexus/3/nexus-3.92.2-01-linux-x86_64.tar.gz

Extract to /opt/
sudo tar xzf nexus-3.92.2-01-linux-x86_64.tar.gz -C /opt/
You'll get two directories: nexus-3.x.x/ and sonatype-work/


Task 4 — Create a dedicated nexus OS user
Nexus should not run as root.

Create user: useradd -M -d /opt/nexus nexus
Set ownership: chown -R nexus:nexus /opt/nexus* /opt/sonatype-work
Tell Nexus to run as that user via nexus.rc


Task 5 — Configure and start Nexus as a service
Make Nexus start automatically and manage it with systemctl.

Create a systemd unit file at /etc/systemd/system/nexus.service
Enable and start the service
Verify it's listening on port 8081


Task 6 — Log in and retrieve the admin password
First login requires a temporary password stored on disk.

Open http://"your-remote-server-ip":8081 in a browser

<img width="1891" height="922" alt="image" src="https://github.com/user-attachments/assets/bd8fda07-0410-4a62-9b09-15c6835d700c" />

Get the initial password from /opt/sonatype-work/nexus3/admin.password

Complete the setup wizard (set a new password, configure anonymous access)

<img width="1919" height="945" alt="image" src="https://github.com/user-attachments/assets/5f1dcd31-d3a7-4312-b028-637749e43c45" />


