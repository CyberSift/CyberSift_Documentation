# Installing the Endpoint Agent on Windows

The edpoint agent uses a combination of tools to export information from a windows computer to cybersift. Installation is straighforward and [Cybersift provides a powershell script](https://github.com/CyberSift/CyberSift_Endpoint_Agents/blob/master/windows/Install_64.ps1) that can be used in a GPO to deploy this across your domain (see caveats below)

## Installation procedure

Download and save the installation powershell script to you harddrive. [The install script can be found here (simply right click and save as...)](https://raw.githubusercontent.com/CyberSift/CyberSift_Endpoint_Agents/master/windows/Install_64.ps1). 

 * Open a powershell shell **with administrator privileges** and change the directory to the powershell script location
 * The script has the following switches:
  * **-server** : (mandatory : string) your CyberSift procesisng server IP address (eg "5.2.3.1")
  * **-Interface** : (mandatory : string) the interface to monitor. Usually a number representing the NIC / Wireless adapter. See below for more information
  * **-Flows** : (optional) monitor network requests to/from this system
  * **-DetectProcess** : (optional) if specified will also attempt to detect which program / process generated a recorded network flow
  * **-Dns** : (optional) monitor DNS requests and responses (enabled you to use the CyberSift DNS Module)
  * **-IncludeBestPractices** : (optional) apart from DNS, process, and flow information, also monitor for other indicators of compromise (at the cost of additional network traffic generated and storage consumed on CyberSift)
  * **-All** : (optional) convenience switch; combination of -Dns -Flows -IncludeBestPractices -DetectProcess

*Please note that **at least** -Dns or -Flows is required
  
## Examples

``powershell
.\Install_64.ps1 -server ec2-5-7-2-1.us-west-2.compute.amazonaws.com -Interface 1 -All
``
  
## Determine the interface number to monitor

* Download a program that depends on libpcap such as [packetbeat](https://github.com/CyberSift/vendor_binaries/raw/master/windows/packetbeat-5.2.2-windows-x86_64.zip) or [WinDump](http://www.winpcap.org/windump/install/default.htm). 
* Get a list of interfaces that can be monitored:
  * **packetbeat**: packetbeat -d
  * **windump** : windump.exe -D
  Use the interface index number as input to the "-Interface" parameter

## Caveats
