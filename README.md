# ssis_script_task_templates
Renci.SshNet.dll has been updated to 2023.0.1
Package sftp_send_receive.dtsx has script that performs connection to sftp server with username, password and port level security. An normal SSIS package connection cannot use credentials

To use this in package the Renci.SshNet.dll has to be added to the GAC. The dll is in Renci folder in the github project. This verison (2020.0.2) is the only one I have managed 
to get to work with VS/SSDT 2022, SSIS C# Script task which is only compatible with 4.7 .Net framework.
I recommend using VS Developer Powershell 7 using these commands:
1. Run VS as an administrator (right click)
2. cd "C:\Program Files (x86)\Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.8.1 Tools"
3. gacutil /i "location of dll\Renci.SshNet.dll"

If you want to get your own build of the dll you can go here and download the Renci's source code on github: https://github.com/sshnet/SSH.NET/releases/tag/2020.0.2
Here are the varibles in ssis and their use. They can be converted to parameters as well but you will need to alter the ScriptMain code for VariableDispenser. All variables are scoped to the Script Task:

bufferSize:			int32, defaulted to 4096
canOverride:		boolean, defaulted to true, will overwrite an existing file(true) or not(false)
doDelete:			boolean, deletes file(s) sent(to hostAddress from localPath) or received(from hostAddress to localPath) after transfer
filePattern:		string, defaulted to *.*, can be set to single file name or used with wildcards(* multiple characters, ? single character)
hostAddress:		string, sftp server name, if blank throws an exception
hostPath:			string, folder location on hostAddress
isReceive:			boolean, true indicates receiving files from hostAddress to localPath, false indicates sending files to hostAddress from localPath
keepAliveInterval:	int32, milliseconds connection is maintained without a response, defaulted to 60, this can be increased if the connection is slow
localPath:			string, location where files are sent or received from hostPath
logFilePath:		string, full path of log file, if left blank no logging will happen. File is replaced every 5 days
passWord:			string, password credential for hostAddress
port:				int32, port on hostAddress
privateKeyFile:		string, not used currently
userName:			string, username credential for hostAddress

Testing can be done by downloading and installing FileZilla: https://filezilla-project.org/ and using a temporary sftp server from this site: https://sftpcloud.io/tools/free-sftp-server



TODO:
1. Use parallel processing (async) to transfer files
2. SSH encryption
3. Private Key connection
