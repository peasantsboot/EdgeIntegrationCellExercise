# Step-by-step installation of Edge Integration Cell

In this exercise, we will follow along with a recorded step-by-step video that takes us through the Edge Integration Cell installation. 

## System pre-requisites

The installation assumes that the following pre-requisites have been met:
1.  First and foremost, we will need privileges to create Kubernetes Service in either EKS, AKS, or SuSE Rancher / RKE2. Please work with your internal IT teams to get hold of these.
2.  Optional - Kubernetes command line tool : kubectl, to dig into some of the internals. 
3.  A technical user (P-User) already exists. The setup process performs an implicit Cloud Connector installation during the Kubernetes bootstrapping process. Pls note that Cloud Connector comes in only for the management plane flows (and not during the actual API / IFlow execution)
4.  An S-user already exists. Using this S-User, a technical user in SAP Repositories Management  https://ui.repositories.cloud.sap/ will be created. This process is needed during the transfer of the code binaries from SAP's central repository to the customer's designated Container Registries.
5.  Optional - To enable central system monitoring, an Identity Authentication Service (IAS) tenant exists.
6.  A private key and a CA-signed certificate that we will use to establish HTTPS on the Istio Ingress Gateway
7.  Control of DNS registry in order to perform an A-record mapping of the Load Balancer IP with the virtual host alias that is established in the step above.

These are only few highlights and by no means an exhaustive list of system prerequisites needed to perform an end-to-end installation. Our help documentation : https://help.sap.com/docs/integration-suite/beta-edge-integration-cell/before-you-start?version=CLOUD  will cover the system prerequisites, supported software versions, network dependencies etc. Please keep an eye on that space 




## Installation Video

Here is the link to the video : <br> <br>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ" target="_blank"><img src="http://img.youtube.com/vi/frIt_8ZsEQQ/0.jpg" alt="IMAGE ALT TEXT HERE" width="700" height="560" border="10" /></a>

## Setup steps

Colons can be used to align columns.

| Step No        | Detail           | Timestamp  |  Video link__________________________ <img width=2000/>|
| -------------- |:----------------| ----------| :------------------------------|
| 1     | <ins>Activate Edge Integration Cell feature </ins> <br><br> In the Integration Suite landing page, you will see there is a new 'Runtime' section within 'Settings'. This section holds the steps to activate the 'Edge Integration Cell' feature. Under the hood, it creates a subscription to a capability called 'Edge Lifecycle Management' (ELM), the management or the control plane that controls the lifecycle of the Edge runtime nodes | 0:0 - 0:24 sec | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ" target="_blank"><img src="/exercises/step-by-step Installation/images/0.png" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a>  |
| 2      | <ins>Setup the Role Collection needed to access the ELM.</ins> <br><br> First, we create a role collection, say 'EdgeLMAccess', and assign the newly available role named 'EdgeLMAccess'. After this, we assign the email address of the technical P-User mentioned in step 3 of the pre-requisites section to this Role Collection. Next, head over to the Cloud Connector Administrator Role Collection and make sure the same user has this assignment as well. These are two mandatory steps. |  0:24 - 1:39 min | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ&t=24s" target="_blank"><img src="/exercises/step-by-step Installation/images/1.png" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a> |
| 3 | <ins>ELM settings - BTP User</ins> <br><br>  Let us get into the ELM administration page. There are certain tenant-level settings that need to be in place before we start adding the Edge Nodes. The first setting is to assign P-user credentials in the SAP Business Technology Platform sub-tab. This will be used by the system to implicitly create the SAP Cloud Connector installation needed to establish the bridge between the management plane and the runtime edge location. |  1:39 - 1:55 min | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ&t=24s" target="_blank"><img src="/exercises/step-by-step Installation/images/2.png" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a> |
| 4 | <ins>ELM settings - RBSC User</ins> <br><br> The next setting is to create a technical user in SAP Repositories Management (https://ui.repositories.cloud.sap/Information published on SAP site) for your logged-in user account. It is used to download SAP software using the Repository-based shipment channel. The technical user that you create is automatically granted access to the repositories based on the licenses of the logged-in user.     |    1:55 - 2:27 min | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ&t=24s" target="_blank"><img src="http://img.youtube.com/vi/frIt_8ZsEQQ/0.jpg" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a> |
| 5 | <ins>ELM settings - Monitor and Log</ins> <br><br> The next setting is to configure the credentials of an IAS (SAP Identity and Authentication Service) tenant. This will be needed to access the Graphana dashboard that we will use to monitor the health and status of the runtime Kubernetes installation. |    2:27 - 2:40 min | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ&t=24s" target="_blank"><img src="http://img.youtube.com/vi/frIt_8ZsEQQ/0.jpg" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a> |
| 6 | <ins>Adding an Edge Node</ins> <br><br>  With the prerequisites now in place, let us start the process of adding an Edge Node (placeholder for the runtime cluster). There are various optional configuration steps here - for example, to specify a local container registry where the binaries will be delivered, HTTPS Proxy details if the Kubernetes nodes are behind one, etc. In this installation, we will stick to the defaults to keep the process simple.     |    2:40 - 3:01 min | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ&t=24s" target="_blank"><img src="http://img.youtube.com/vi/frIt_8ZsEQQ/0.jpg" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a> |
| 7 | <ins>Kubernetes setup in AKS</ins> <br><br> Now the wizard is at a step where we will need the details of the Kubernetes configuration where the edge cell installation will be done, i.e. the kubeconfig file. For our installation, we will use AKS (Azure Kubernetes Service) as the hyperscaler of choice. Note that there are other possibilities here as well. To keep things simple, we will follow to the easiest path of getting a Kubernetes cluster setup. Note that in reality there could be many network-related steps here based on your local IT governance requirements. The important aspects here would be to select the right supported version of Kubernetes service, the size of the machines (minimum 8 vCPU, 32 GB RAM), and expandable node pools. Once the installation is complete, we launch the inbuilt Cloud Shell to get hold of the kubeconfig file of the provisioned cluster.     |    3:01 - 4:06 min | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ&t=24s" target="_blank"><img src="http://img.youtube.com/vi/frIt_8ZsEQQ/0.jpg" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a> |
| 8 | <ins>Download bootstrapping file</ins> <br><br> The kubeconfig file will be needed to set up the local kubectl command line, for a Kubernetes administrator. Let us now get back to the 'Edge Node' installation steps. Based on our inputs, the system will generate a context.cfg file that will be used for the next set of steps. We will also download the ELMB binary from SAP Service Marketplace, mount it to our MacOS, and pull the installer out from the binaries.      |    4:06 - 4:47 min | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ&t=24s" target="_blank"><img src="http://img.youtube.com/vi/frIt_8ZsEQQ/0.jpg" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a> |
| 9 | <ins>Execute the ELM Bridge bootstrapping commands</ins> <br><br> With the  ELM executable and the context.cfg file, we start the bootstrapping process from our local machine (that has access to the API Server of the Kubernetes installation). This is supported only via a command line interface. The wizard will run through a series of steps, the principal ones being the installation and configuration of the cloud connector and the 'Edge Lifecycle Management' base solution installation.     |    4:47 - 5:37 min | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ&t=24s" target="_blank"><img src="http://img.youtube.com/vi/frIt_8ZsEQQ/0.jpg" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a> |
| 10 | <ins>Manage keypairs for SSL handling</ins> <br><br>  The next set of steps is to install the edge integration cell solution itself. We start by creating an RSA keypair, needed to configure TLS and the HTTPS handshake on the Edge Ingress Gateway (we will use Istio as the Ingress Gateway layer). The keypair is encoded as eicdemo.pfx, a file that is imported into the security section of the SAP Integration Suite's. Note that the certificate has been issued for a wildcard (*) domain: sapintegrationsuite.de.    |   5:37 - 6:44 min | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ&t=24s" target="_blank"><img src="http://img.youtube.com/vi/frIt_8ZsEQQ/0.jpg" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a> |
| 11 | <ins>Edge Integration Cell solution deployment</ins> <br><br>  Let us now deploy hte Edge Integration Cell solution. The installation wizard has a configuration section where we could define the sizing of the cluster, bring in our own persistence and storage classes, etc. To keep things simple, we will stick to the defaults. Once this step successfully completed, the solution and the dependent components are installed and ready to execute.    |   6:44 - 7:12 min | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ&t=24s" target="_blank"><img src="http://img.youtube.com/vi/frIt_8ZsEQQ/0.jpg" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a> |
| 12 | <ins>configuring kubectl tool</ins> <br><br>      |    7:12 - 7:44 min | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ&t=24s" target="_blank"><img src="http://img.youtube.com/vi/frIt_8ZsEQQ/0.jpg" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a> |
| 13 | <ins>Runtime profile verification</ins> <br><br>     |    7:44 - 8:00 min | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ&t=24s" target="_blank"><img src="http://img.youtube.com/vi/frIt_8ZsEQQ/0.jpg" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a> |
| 14 | <ins>DNS mapping of virtual host alias</ins> <br><br>      |    8:00 min onwards | <a href="http://www.youtube.com/watch?feature=player_embedded&v=frIt_8ZsEQQ&t=24s" target="_blank"><img src="http://img.youtube.com/vi/frIt_8ZsEQQ/0.jpg" alt="IMAGE ALT TEXT HERE" width="350" height="280" border="10" /></a> |

There must be at least 3 dashes separating each header cell.
The outer pipes (|) are optional, and you don't need to make the 
raw Markdown line up prettily. You can also use inline Markdown.

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3

## Summary

You've now ...

Continue to - [Exercise 2 - Exercise 2 Description](../ex2/README.md)
