**Server Monitoring Solution**

This repository provides a complete solution for monitoring multiple servers using Prometheus, Grafana, and Alertmanager with email alerting. The setup includes Node Exporter running on each server to collect system metrics, which are then visualized in Grafana and monitored by Prometheus.
________________________________________

**Step 1:** _Clone the Repository_

git clone <repository-url

cd Server-Monitoring-Solution

________________________________________

**Step 2:** _Set Up Node Exporter on Each Server_

1.	Copy the setup_node_exporter.sh script to each server you want to monitor.

2.	Make the script executable and run it using the following commands:

chmod +x setup_node_exporter.sh

./setup_node_exporter.sh

**This script will:**
**o**	Install Docker if it’s not already installed.

**o**	Install Docker Compose if it’s not already installed.

**o**	Add the current user to the Docker group to avoid using sudo for Docker commands.

**o**	Set up and run Node Exporter in a Docker container.
________________________________________

**Step 3:** _Set Up Central Monitoring_

**1.**	Navigate to the Central_Monitoring_Solution directory:

cd Central_Monitoring_Solution

**2.**	Ensure that the prometheus.yml file has the correct targets for your Node Exporter instances:

static_configs:
  - targets:
      - '13.60.254.27:9100'  # Test 1
      - '13.61.143.220:9100'  # Test 2

**3.**	Modify the alertmanager.yml file with your email configuration:

smtp_smarthost: 'smtp.gmail.com:587' # this will remain same for Gmail only if you use your gmail as smtp

smtp_from: 'your-email@gmail.com'

smtp_auth_username: 'your-email@gmail.com'

smtp_auth_password: 'your-app-specific-password'
________________________________________

**Step 4:** _Start Prometheus, Grafana, and Alertmanager_
**1.**	Start all services using Docker Compose:

sudo docker-compose up -d

**2.**	Access the services in your browser:

o	Prometheus: http://<central-monitoring-server-IP:9090

o	Grafana: http://<central-monitoring-server-IP:3000

o	Alertmanager: http://<central-monitoring-server-IP:9093
________________________________________

**Step 5:** _Verify Setup_

**1.**	Prometheus should display the Node Exporter instances under Status → Targets.

**2.**	Grafana should be configured to visualize metrics from Prometheus.

**3.**	Alertmanager should send email notifications when an alert is triggered (e.g., when a Node Exporter instance goes down).
________________________________________

Testing the Setup

To test email alerts, try stopping one of the Node Exporter containers on a monitored server:

sudo docker stop node_exporter

Wait for 1-2 minutes, and you should receive an email notification from Alertmanager indicating that the instance is down. Once you restart the Node Exporter container, you should receive a resolution email.
