# Deploy AWS to Amazon Web Services Cloud

## Overview
TODO

## Procedure

1. Create a new Managed Elasticsearch Service. Select the hosted elasticsearch option in AWS.

![aws-hosed-es](https://docs.google.com/drawings/d/1QvRwJ7rMoQ3CRl1HLMsRJuvVm6amx0WOoBP30gpPjSg/pub?w=778&h=145)

2. Click on **“create a new domain”**, enter a name and select elasticsearch version 5.1. In this case, we’ll name our managed instance *“cs-cloud-demo”*:

![es-domain](https://docs.google.com/drawings/d/1_pbq8I5Vw-JI0rO5fNBKd3XeBBI7cDyP4ns-AqQl_7s/pub?w=875&h=611)

3. Configure the size of your cluster. Sizing depends on your environment, please browse our sizing guides (TODO) for more information. For the purposes of this guide we choose to run only 1 instance of type “small” with 10GB of storage. This should be sufficient for demos and very small setups

![es-domain-settings](https://docs.google.com/drawings/d/1w_vVfW-ckPHbTZY_T5pAlEHgSIQ_BGSupznGLfcDkCA/pub?w=756&h=680)

4. Setup an access policy. We recommend at the very least setting up an IP address policy that restricts access to the elasticsearch domain to just your authorized IP addresses. Later on, we can close all access to the elasticsearch domain except for the CyberSift processing server IP address, making this a bit more secure. To start with, enter your public IP address here:

![aws-es-access-policy](https://docs.google.com/drawings/d/1SyXgmHyVHSG6CnejC0IjCRA4iBGMyKFQ8rJWuAY2UoM/pub?w=899&h=518)

5. Review the generated settings and you’re done! Next we setup the CyberSift processing server.

![aws-es-review](https://docs.google.com/drawings/d/1uqOf9zEhMUOcjL5qWxqamduppHYBosYdQr9Rs9YgVQY/pub?w=918&h=2540

6. Launch the CyberSift AWS AMI. The AMI can be found in the EC2 dashboard under Images > AMIs (please note that you need to contact CyberSift support to gain access to this AMI - it is not publically available yet). Right click and “launch” the AMI. When reviewing the settings, we recommend allowing only the following ports, only from authorized public IPs:

 * SSH (port 22)
 * Management SSH (please contact CyberSift Support)
 * HTTP (port 80) 
 
 Once the AMI has been launched, make a note of the public IP as shown below:
 
 ![aws-ec2-public-ip](https://docs.google.com/drawings/d/1q40e6QMZpqEmpUO9P9AAF_UM0ToAv8j4V8kfvw0wNdk/pub?w=499&h=100)
 
 7. Add the above IP to the elasticsearch access policy. This will take a while to process, so please make sure that the domain status transitions successfully from "processing" to "active" as shown below
 
 ![ec2-es-ap](https://docs.google.com/drawings/d/1m0HqlJykjOf8S8Oa1yG5MvQNHyIFviT9MVDUGwntfMo/pub?w=578&h=491)
 
 
