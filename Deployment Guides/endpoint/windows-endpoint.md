# Installing the Endpoint Agent on Windows

The edpoint agent uses a combination of tools to export information from a windows computer to cybersift. Installation is straighforward and [Cybersift provides a powershell script](https://github.com/CyberSift/CyberSift_Endpoint_Agents/blob/master/windows/Install_64.ps1) that can be used in a GPO to deploy this across your domain (see caveats below)

## Installation procedure

Download and save the installation powershell script to you harddrive. [The install script can be found here (simply right click and save as...)](https://raw.githubusercontent.com/CyberSift/CyberSift_Endpoint_Agents/master/windows/Install_64.ps1). Open a powershell shell **with administrator privileges** and change the directory to the powershell script location

## Caveats
