# Google-compute-engine-Ops-Agent
Monitoring a Compute Engine by using Ops Agent

>>>>> # Create a Compute Engine VM instance #<<<<<<<<
>>>>> 1./In Google Cloud console, go to Compute and then select Compute Engine.

2./To create a VM instance, click Create instance.

3./Fill in the fields for your instance as follows:

In the Name field, enter quickstart-vm.
In the Machine type field, select e2-small.
Ensure the Boot disk is configured for Debian GNU/Linux.
In the Firewall field, select both Allow HTTP traffic and Allow HTTPS traffic.

Install an Apache Web Server

To deploy an Apache Web Server on your Compute Engine VM instance, do the following:

1./To open a terminal to your instance, in the Connect column, click SSH.

2./To update the package lists on your instance, run the following command:

 sudo apt-get update

3./To install an Apache2 HTTP Server, run the following command:
  sudo apt-get install apache2 php7.0

4./ Open your browser and connect to your Apache2 HTTP server by using the URL http://EXTERNAL_IP, where EXTERNAL_IP is the external IP address of your VM. You can find this address in the External IP column of your VM instance.

  >>>> Install and configure the Ops Agent <<<<

To collect logs and metrics from your Apache Web Server, install the Ops Agent by using the terminal:


1./To open a terminal to your VM instance, in the Connect column, click SSH.

2./To install the Ops Agent, run the following command:

curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh
sudo bash add-google-cloud-ops-agent-repo.sh --also-install

3./Copy the following command, then paste it into the terminal:
# Configures Ops Agent to collect telemetry from the app and restart Ops Agent.

set -e

# Create a back up of the existing file so existing configurations are not lost.
sudo cp /etc/google-cloud-ops-agent/config.yaml /etc/google-cloud-ops-agent/config.yaml.bak

# Configure the Ops Agent.
sudo tee /etc/google-cloud-ops-agent/config.yaml > /dev/null << EOF
metrics:
  receivers:
    apache:
      type: apache
  service:
    pipelines:
      apache:
        receivers:
          - apache
logging:
  receivers:
    apache_access:
      type: apache_access
    apache_error:
      type: apache_error
  service:
    pipelines:
      apache:
        receivers:
          - apache_access
          - apache_error
EOF

sudo service google-cloud-ops-agent restart
sleep 60

//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Google cloud documentation <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
>>>>>>>>>>>>>>>>>>>>>>>>>>> Google cloud requirements <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
>>>>>>>>> https://cloud.google.com/logging/docs/agent/ops-agent/third-party/apache <<<<<<<<<
