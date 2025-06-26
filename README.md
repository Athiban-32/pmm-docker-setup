# pmm-docker-setup
Install Percona Monitoring and Management on Docker

<h2>
Step 1: Install PMM Server
</h2>


curl -fsSL https://www.percona.com/get/pmm | /bin/bash --> if it not works use below command

1.sudo dnf install -y docker

2.sudo systemctl start docker

3.sudo usermod -aG docker ec2-user

4.curl -fsSL https://www.percona.com/get/pmm


<h2>
Step 2: Install PMM Client
</h2>

1. Install Percona Release Repository:
sudo yum install -y https://repo.percona.com/yum/percona-release-latest.noarch.rpm

2. Install PMM Client:
sudo percona-release enable pmm3-client
sudo yum install -y pmm-client

<h2>
Step 3: Connect PMM Client to Server
</h2>

Register the Client with your PMM Server, replacing `<PMM_SERVER_IP>` with your PMM Server’s IP address or hostname:

pmm-admin config --server-insecure-tls --server-url=https://admin:<admin_password>@<PMM_SERVER_IP>:443

<h2>
Step 4: Create a PMM user for monitoring
</h2>

1. Run the following SQL commands to create a dedicated user for PMM monitoring: For MySQL 8.0:
CREATE USER 'pmm'@'127.0.0.1' IDENTIFIED BY '<your_password>' WITH MAX_USER_CONNECTIONS 10;
GRANT SELECT, PROCESS, REPLICATION CLIENT, RELOAD, BACKUP_ADMIN ON *.* TO 'pmm'@'127.0.0.1';

For MySQL 5.7:
CREATE USER 'pmm'@'127.0.0.1' IDENTIFIED BY '<your_password>' WITH MAX_USER_CONNECTIONS 10;
GRANT SELECT, PROCESS, REPLICATION CLIENT, RELOAD, SUPER ON *.* TO 'pmm'@'127.0.0.1';

2. Enable monitoring:
pmm-admin add mysql --query-source=perfschema --username=pmm --password=<your_password>

