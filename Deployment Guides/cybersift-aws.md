# Deploy AWS to Amazon Web Services Cloud

## Overview
In this deployment guide we will go through provisioning a complete CyberSift installation in AWS cloud. The general architecture will look as follows:

![cs-aws-overview](https://docs.google.com/drawings/d/1sM1wzGNCuBX9Yn0IKkWQ6ArDqJ5IaD-owGavwFPfoVI/pub?w=960&h=720)

## Procedure

1. Create a new Managed Elasticsearch Service. Select the hosted elasticsearch option in AWS.

![aws-hosed-es](https://docs.google.com/drawings/d/1QvRwJ7rMoQ3CRl1HLMsRJuvVm6amx0WOoBP30gpPjSg/pub?w=778&h=145)

2. Click on **“create a new domain”**, enter a name and select elasticsearch version 5.1. In this case, we’ll name our managed instance *“cs-cloud-demo”*:

![es-domain](https://docs.google.com/drawings/d/1_pbq8I5Vw-JI0rO5fNBKd3XeBBI7cDyP4ns-AqQl_7s/pub?w=875&h=611)

3. Configure the size of your cluster. Sizing depends on your environment, please browse our sizing guides (TODO) for more information. For the purposes of this guide we choose to run only 1 instance of type “small” with 10GB of storage. This should be sufficient for demos and very small setups

![es-domain-settings](https://docs.google.com/drawings/d/1w_vVfW-ckPHbTZY_T5pAlEHgSIQ_BGSupznGLfcDkCA/pub?w=756&h=680)

4. Setup an access policy. We recommend at the very least setting up an IP address policy that restricts access to the elasticsearch domain to just your authorized IP addresses. Later on, we can close all access to the elasticsearch domain except for the CyberSift processing server IP address, making this a bit more secure. To start with, enter your public IP address here:

![aws-es-access-policy](https://docs.google.com/drawings/d/1SyXgmHyVHSG6CnejC0IjCRA4iBGMyKFQ8rJWuAY2UoM/pub?w=899&h=518)

5. Review the generated settings and you’re done! Make a note of the endpoint url because we'll need it next when we setup the CyberSift processing server.

![aws-es-review](https://docs.google.com/drawings/d/1uqOf9zEhMUOcjL5qWxqamduppHYBosYdQr9Rs9YgVQY/pub?w=918&h=2540

6. Launch the CyberSift AWS AMI. The AMI can be found in the EC2 dashboard under Images > AMIs (please note that you need to contact CyberSift support to gain access to this AMI - it is not publically available yet). Right click and “launch” the AMI. When reviewing the settings, we recommend allowing only the following ports, only from authorized public IPs:

 * SSH (port 22)
 * Management SSH (please contact CyberSift Support)
 * HTTP (port 80) 
 
 Once the AMI has been launched, make a note of the public IP as shown below:
 
 ![aws-ec2-public-ip](https://docs.google.com/drawings/d/1q40e6QMZpqEmpUO9P9AAF_UM0ToAv8j4V8kfvw0wNdk/pub?w=499&h=100)
 
 7. Add the above IP to the elasticsearch access policy. This will take a while to process, so please make sure that the domain status transitions successfully from "processing" to "active" as shown below
 
 ![ec2-es-ap](https://docs.google.com/drawings/d/1m0HqlJykjOf8S8Oa1yG5MvQNHyIFviT9MVDUGwntfMo/pub?w=578&h=491)
 
 ![es-domain-processing](https://docs.google.com/drawings/d/1K6RJoyzV6FZkrjyYC8XUDOj0_vGiRr3NLjLUY68Wo8w/pub?w=274&h=134)
 
 8. We can now open the CyberSift dashboard at "http://enter-your-ec2-public-ip". The initial dashboard will look similar to the below:
 
 ![cs-initial-dashboard](https://docs.google.com/drawings/d/1lVv8dAV1LUS7wS8z9GI7iHLSOkv2lLSBpXt7AESXmCk/pub?w=924&h=392)
 
 Notice the empty vizualizations and the widgets displaying "N/A". This is because we now need to perform the last step - connecting the CyberSift processing server to the ElasticSearch backend. This is done very easily by using the CyberSift dashboard to navigate to: **Configuration > Distributed Architecture Setup**.  
 
 First, copy/paste the ElasticSearch endpoint displayed in step 5 above, into the **ElasticSearch REST API** configuration box. **Important: Make sure to click "Save Settings"!** 
 
 ![cs-connect-es](https://docs.google.com/drawings/d/1bqa0CsPsiPKOpSCUQ1nFzheFtqoXRLLA73hOfK7bhDQ/pub?w=812&h=680)
 
 Second, expand **Cloud Settings** and press **Provision New ElasticSeach Backend** *(note this may take about 30-45 seconds to complete)*
 
 ![cs-cloud-settings](https://docs.google.com/drawings/d/1Pq5L4MSvoX4SQMf1Ac2bX657E4u-kXWZ14W0jvbj7bk/pub?w=365&h=281)
 

**All done!** We're ready to start sending logs to CyberSift!. Some elements of the UI may appear blank or unresponsive until you send some logs to Cyberift. Let's quickly test ssending some information to CyberSift to ensure everything is working as it should

### Generate some honeypot alerts.

One of the easiest ways to gnerate logs in CyberSift is to trigger the in-built honeypot. Simply try to SSH to CyberSift's public IP (i.e. the EC2 instance public IP). After logging in successfully, refresh the CyberSift dashboard and you should see something similar to the below:

![cs-dashboard-honeypot](https://docs.google.com/drawings/d/1ZRJIOdCS0QMnwEg83S7DZXQ-v6RggHnk_sM8WlzKyzQ/pub?w=929&h=100)

This quick test shows that CyberSift is accepting logs, and also that we have successfully connected to our backend ElasticSearch service.

### Send some endpoint logs to cybersift

Another simple way of testing cybersift is to start sending some endpoint logs to elasticsearch. You can try this with a windows system. Simply follow the installation procedure outlined here:

[Installing the Endpoint Agent on Windows](https://github.com/CyberSift/CyberSift_Documentation/blob/master/Deployment%20Guides/endpoint/windows-endpoint.md)

After successfully installing the agent, you can click on the **CyberSift Logging Engine** item in the CyberSift dashboard. This will take you to the Kibana backend that powers CyberSift. 

**Note:** Since this is probably the first time running CyberSift, you need to refresh the indices to ensure Kibana will display the correct information. To do this, click on "Management > Index Patterns > packetbeat-\*" and click on the refresh icon, as shown below. Repeat this for filebeat-\*

![cs-refresh-index](https://docs.google.com/drawings/d/1qztmpOz02rD09wX9HZ9qZawxpxG-NhQcqLXo5sQjbdw/pub?w=926&h=276)

All done! Now in the "Discover" tab, indices should start being populated, as shown below:

![cs-populated-index](https://docs.google.com/drawings/d/1YxPwOAKxjXnTmxiKGuBMIgIPOpsK6F62_ayb2P5QN3w/pub?w=925&h=204)

Happy Threat Hunting!

