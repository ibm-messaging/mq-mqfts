# IBM MQ Managed File Transfer Log Capture Parser

IBM MQ Managed File Transfer (MFT) transfers files between systems in a managed and auditable way, regardless of file size or the operating systems used. You can use Managed File Transfer to build a customized, scalable, and automated solution that enables you to manage, trust, and secure file transfers. Managed File Transfer eliminates costly redundancies, lowers maintenance costs, and maximizes your existing IT investments.

Refer [IBM MQ Managed File Transfer](https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.2.0/com.ibm.mq.pro.doc/wmqfte_intro.html) Knowledge Center for more details.

IBM MQExplorer MFT Plugin and MFT Logger are the tools provided by MFT for tracking transfers. These two tools can be used to track transfers that are happening across MFT network. MFT provides an attribute called `logCapture` in agent.properties file. Enabling this attribute allows agent to capture transfer request messages that are submitted to this agent and log messages that are published by the agent to the coordination queue manager. These captured messages can be helpful when debugging transfer problems. Captured messages are stored in files in the agent log directory called capture?.log. The ? is a numeric value. The file that contains the number 0 holds the newest captured messages

The captured messages are XML formatted. These XML messages may be difficult to read through and determine the status of any transfer, especially if the log file contains lot of messages. 

### IBM MQ Managed File Transfer Log Capture Parser Utility (MQFTS)
MQFTS is a utility that parses the transfer XML messages from the captureN.log file and displays the transfer information in an easy-to-read format on the console. The captureN.log files may contain other XML messages, like Resource Monitor messages. However the utility parses only the transfer XML messages.

### How to use MQFTS
The utility can be run by executing `mqfts` command on the command line. The following is the help panel of the command.
```
IBM MQ Managed File Transfer Status Utility
Usage:
    mqfts       Display status of transfers present in log file
    mqfts <--lf>=<capture log file>
    mqfts <--id>=<Transfer ID> Display details of single transfer. Specify * for all transfers
    mqfts <--fl>=<n> Display recent <n> failed transfers
    mqfts <--sf>=<n> Display recent <n> successful transfers
    mqfts <--ps>=<n> Display recent <n> partially successful transfers
    mqfts <--st>=<n> Display recent <n> transfers in 'started' state
    mqfts <--ip>=<n> Display recent <n> 'In Progress' transfers
```
There are couple of ways the utility can be used. 
##### By specifying path of captureN.log file
You can pass the complete path of the capture log file with `--lf` parameter

`mqfts --lf=/var/mqm/mqft/logs/capture0.log`

##### By setting environment variables 
Instead of specifiying log file name with --lf parameter everytime, you can set the following environment variables before running the mqfts command. Using the environment variables, the utility generates the appropriate path of a log file. 
```
export BFG_DATA=</var/mqm/mqft> - Path of MFT configuration directory
export MFT_COORDINATION_QM=<name of your coordination qm>
export MFT_AGENT_NAME=<agent name>
```

Then simply executing `mqfts` command will list transfer status from an agents capture0.log file.
 
Utility also provides a number of options, for example: Display
  1) Only failed or partial transfers. 
  2) Full details of a single transfer or all transfers
  3) List of last 'N' failed or successful transfers
 
### How to build the utility
The source code for the utility has been provided. The source is in golang and requires few third party golang libraries from GitHub. The utility has been compiled with go1.14.6

1) Install golang SDK.
2) Clone the repository to any directory.
3) Change directory to where source is located.
4) Ensure path go compiler is set in the environment. For example `export PATH=$PATH:/usr/go/bin` assuming go SDK is installed in `/usr/go` directory.
5) Run `go build`.

