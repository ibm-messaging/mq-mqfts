# IBM MQ Managed File Transfer Log Capture Parser

IBM MQ Managed File Transfer (MFT) transfers files between systems in a managed and auditable way, regardless of file size or the operating systems used. You can use Managed File Transfer to build a customized, scalable, and automated solution that enables you to manage, trust, and secure file transfers. Managed File Transfer eliminates costly redundancies, lowers maintenance costs, and maximizes your existing IT investments.

Refer [IBM MQ Managed File Transfer](https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.2.0/com.ibm.mq.pro.doc/wmqfte_intro.html) Knowledge Center for more details.

IBM MQExplorer MFT Plugin and MFT Logger are the tools provided by MFT for tracking transfers. These two tools can be used to track transfers that are happening across MFT network. MFT provides an agent attribute 'logCapture' to log transfer details to a log file in the agent's log directory. Enabling this attribute allows agent to capture transfer request messages that are submitted to this agent and log messages that are published by the agent to the coordination queue manager. These captured messages can be helpful when debugging transfer problems. Captured messages are stored in files in the agent log directory called capture?.log. The ? is a numeric value. The file that contains the number 0 holds the newest captured messages

The captured messages are XML formatted. These XML messages may be difficult to read through and determine the status of any transfer, especially if the log file contains lot of messages. 

### IBM MQ Managed File Transfer Log Capture Parser Utility (MQFTS)
MQFTS is a utility that parses the capture XML messages and displays the transfer information in a easy-to-read format on the console. The captureN.log files may contain other XML messages, like Resource Monitor messages. The utility parses only the transfer XML messages.

### How to use?
The utility can be run by executing 'mqfts' command on the command line. The following is the help panel of the command.
![image](mqfts.png)

There are couple of ways the utility can be used. 
#### By specifying path of captureN.log file
You can pass the complete path of the capture log file with '--lf' parameter
mqfts --lf=/var/mqm/mqft/logs/capture0.log

#### By setting environment variables 
Instead of specifiying log file name with --lf parameter everytime, you can set the following environment variables before running the mqfts command. Using the environment variables, the utility generates the appropriate path of a log file. 
export BFG_DATA=/var/mqm/mqft
export MFT_COORDINATION_QM=<name of your coordination qm>
export MFT_AGENT_NAME=<agent name>

Then simply executing 'mqfts' command will list transfer status from an agents capture0.log file.
 
Utility also provides a number of options, for example: Display
  1) Only failed or partial transfers. 
  2) Full details of a single transfer or all transfers
  3) List of last 'N' failed or successful transfers
 
### How to build the utility
The source code for the utility has been provided. The source is in golang and requires few third party golang libraries from GitHub. The utility has been compiled with go1.14.6

