# Getting Started ABCI

## Getting an ABCI account {#getting-an-account}

### Application for use of ABCI
To use an ABCI account, determine the research and development theme according to the agreements of use (rules or regulations) posted in the "Application Procedure", and submit a new "ABCI Application" on the ABCI User Portal. The ABCI application will be reviewed based on the terms of use, and if requirements are met, an ABCI group for which the application was made will be created and the applicant will be notified that the application has been adopted. After receiving notification, the applicant may start using the account.

Please refer to the price list in [ABCI USAGE FEES](https://abci.ai/en/how_to_use/tariffs.html) and purchase ABCI points to pay the usage fee. Please purchase ABCI points for each ABCI group. At the time of application, a minimum of 1,000 ABCI points must be purchased.

### Types of ABCI accounts {#account-type}
There are three types of ABCI accounts: "Responsible Person", "User Administrator", "User".
To use the ABCI system, the "Responsible Person" must submit a new "ABCI application" on the [ABCI User Portal](https://portal.v3.abci.ai/ProjectApplication_01) and obtain an ABCI group and one or more ABCI accounts. See the [ABCI Portal Guide](https://docs.abci.ai/v3/portal/ja/) for more details.

!!! Note
    - An ABCI account will also be issued to the "Responsible Person".
    - The "Responsible Person" can change "User" to "User Administrator" on the [ABCI User Portal](https://portal.abci.ai/user/).
    - The "Responsible Person" and "User Administrator" can add "User Administrators" or "Users" to the ABCI group.
    - The "Responsible Person" and "User Administrator" can change the "Responsible Person" of the ABCI group.
    - Currently, ABCI 3.0 User Portal and the portal guide are available only in Japanese.

### Uniqueness of the ABCI account {#account-uniqueness}
As a general rule, each person should have a unique ABCI account. For example, it is not allowed that a company gets one ABCI account with use shared among more than one employees. On the other hand, if one person belongs to multiple corporations, that person may obtain multiple ABCI accounts. In such cases, the ABCI account corresponding to each corporation must be used appropriately.

### Persons belonging to multiple ABCI groups {#multi-titled-person}
If one person is working on more than one themes in one corporation at the same time, instead of that person acquiring multiple ABCI accounts, such a person will belong to multiple ABCI groups with one ABCI account. In such cases, each ABCI group must be used appropriately according to the purpose of use. When unsure of which ABCI group to use, please contact the "Responsible Person" or "User Administrator" of the ABCI group. The "Responsible Person" or "User Administrator" of the ABCI group to which you belong will be displayed on the first screen after logging in to the [ABCI User Portal](https://portal.abci.ai/user/).

Since usage determines which ABCI group bears the usage fee, please use the appropriate ABCI group according to the instructions of the "Responsible Person" or "User Administrator" of the ABCI group.

## Connecting to Interactive Node

To connect to the interactive node (*login*), the ABCI frontend, two-step SSH public key authentication is required.

1. Login to the access server (*as.v3.abci.ai*) with SSH public key authentication, so as to create an *SSH tunnel* between your computer and *login*.
2. Login to the interactive node (*login*) with SSH public key authentication via the SSH tunnel.

In this document, ABCI server names are written in *italics*.

### Prerequisites

To connect to the interactive node, you will need the following in advance:

* SSH client: Your computer most likely has an SSH client installed by default. If your computer is a UNIX-like system such as Linux and macOS, or Windows 10 version 1803 (April 2018 Update) or later, it should have an SSH client. You can also check for an SSH client, just by typing ``ssh`` at the command line.
* SSH protocol version: Only SSH protocol version 2 is supported.
* A secure SSH public/private key pair: ABCI only accepts the following public keys:
	* RSA keys, at least 2048bits
	* ECDSA keys, 256, 384, and 521bits
	* Ed25519 keys
* Please register your SSH public key through the [ABCI 3.0 User Portal](https://portal.v3.abci.ai/).
* For detailed instructions on how to use the ABCI 3.0 User Portal, please refer to the following guide:
	* User Guide: [https://docs.abci.ai/v3/portal/en/](https://docs.abci.ai/v3/portal/en/)

!!! note
    Tera Term and PuTTY can be used as SSH clients.

### Login using an SSH Client

In this section, we will describe two methods to login to the interactive node using a SSH client. The first one is creating an SSH tunnel on the access server first and connecting the interactive node via this tunnel next. The second one, much easier method, is connecting directly to the interactive node using ProxyJump implemented in OpenSSH 7.3 or later.

#### General method

Login to the access server (*as.v3.abci.ai*) with following command. As an example, the port number is set to 50022. However, if this port is already in use for SSH connections to other hosts or for network services on your local PC, please change it accordingly.

```
[yourpc ~]$ ssh -i /path/identity_file -L 50022:login:22 -l username as.v3.abci.ai
The authenticity of host 'as.v3.abci.ai (0.0.0.1)' can't be established.
RSA key fingerprint is XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX. <- Display only at the first login
Are you sure you want to continue connecting (yes/no)? <- Enter "yes"
Warning: Permanently added 'XX.XX.XX.XX' (RSA) to the list of known hosts.
Enter passphrase for key '/path/identity_file': <- Enter passphrase
```


Successfully logged in, the following message is shown on your terminal.

```
Welcome to ABCI access server.
Please press any key if you disconnect this session.
```

!!! warning
    Be aware! The SSH session will be disconnected if you press any key.

Launch another terminal and login to the interactive node using the SSH tunnel:

```
[yourpc ~]$ ssh -i /path/identity_file -p 50022 -l username localhost
The authenticity of host 'localhost (127.0.0.1)' can't be established.
RSA key fingerprint is XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX. <- Display only at the first login
Are you sure you want to continue connecting (yes/no)? <- Enter "yes"
Warning: Permanently added 'localhost' (RSA) to the list of known hosts.
Enter passphrase for key '/path/identity_file': <- Enter passphrase
[username@login1 ~]$
```

#### ProxyJump

You can log in to an interactive node with a single command using ProxyJump, which was introduced in OpenSSH version 7.3. ProxyJump can be used in Windows Subsystem for Linux (WSL) environment as well.

First, add the following configuration to your ``$HOME/.ssh/config``:

```
Host abci
     HostName login
     User username
     ProxyJump %r@as.v3.abci.ai
     IdentityFile /path/to/identity_file
     HostKeyAlgorithms ssh-ed25519

Host as.v3.abci.ai
     IdentityFile /path/to/identity_file
```

After that, you can log in with the following command only:

```
[yourpc ~]$ ssh abci
```

ProxyJump does not work with OpenSSH_for_Windows_7.7p1 which is bundled with Windows 10 version 1803 and later. Use ProxyCommand instead. The following is an example of a config file using ProxyCommand. Please specify the absolute path for `ssh.exe`.

```
Host abci
     HostName login
     User username
     ProxyCommand C:\WINDOWS\System32\OpenSSH\ssh.exe -W %h:%p %r@as.v3.abci.ai
     IdentityFile C:\path\to\identity_file

Host as.v3.abci.ai
     IdentityFile C:\path\to\identity_file
```

## File Transfer to Interactive Node

When you transfer files between your computer and the ABCI system, create an SSH tunnel and run the `scp` (`sftp`) command.

```
[yourpc ~]$ scp -P 50022 local-file username@localhost:remote-dir
Enter passphrase for key: <- Enter passphrase
    
local-file    100% |***********************|  file-size  transfer-time
```

If you have OpenSSH 7.3 or later and already added the configuration to your ``$HOME/.ssh/config`` as described at [ProxyJump](#proxyjump), you can directly run the `scp` (`sftp`) command.

```
[yourpc ~]$ scp local-file abci:remote-dir
```

## Login Shell

The default login shell for the ABCI system is set to bash. For any changes to the login shell, please contact the operations team (abci3-qa@abci.ai ).


## Checking ABCI Point

To display ABCI point usage and limitation, use the `show_point` command.
When your ABCI point usage ratio will reach 100%, a new job cannot be submitted, and queued jobs will become error state at the beginning. (Any running jobs are not affected.)

Example) Display ABCI point information.

```
[username@login1 ~]$ show_point
Group                 Disk            ObjectStorage                    Used           Point   Used%
grpname                  5                  0.0124             12,345.6789         100,000      12
  `- username          -                         -                  0.1234               -       0
 
```

| Item | Description |
|:--|:--|
| Group | ABCI group name |
| Disk  | Disk assignment (TB) |
| ObjectStorage | ABCI point usage of ABCI Object Storage |
| Used  | ABCI point usage |
| Point | ABCI point limit |
| Used% | ABCI point usage ratio |

To display ABCI point usage per monthly, use the `show_point_history` command.

Example) Display ABCI point usage per monthly.

```
[username@login1 ~]$ show_point_history -g grpname
                      Apr        May        Jun        Jul        Aug        Sep        Oct        Nov        Dec        Jan        Feb        Mar          Total
Disk           1,000.0000     0.0000     0.0000          -          -          -          -          -          -          -          -          -     1,000.0000
ObjectStorage       1.0000     1.5000     0.5000          -          -          -          -          -          -          -          -          -         2.0000
Job              100.0000    50.0000    10.0000          -          -          -          -          -          -          -          -          -       160.0000
  |- username1    60.0000    40.0000     5.0000          -          -          -          -          -          -          -          -          -       105.0000
  `- username2    40.0000    10.0000     5.0000          -          -          -          -          -          -          -          -          -        55.0000
Total          1,101.0000    51.5000    10.5000          -          -          -          -          -          -          -          -          -     1,162.0000
```

| Item | Description |
|:--|:--|
| Disk  | ABCI point usage of Disk |
| ObjectStorage | ABCI point usage of ABCI Object Storage |
| Job  | Total ABCI point usage of On-demand, Spot, and Reserved Services for all users in the group |
| Total(rows) | Total ABCI point usage of Disk, ObjectStorage, and Job |

!!! note
    - For information on calculating point consumption per service, see [Accounting](job-execution.md#accounting).
    - The point usage of the job of Spot/On-demand service which executed across months is counted in the month in which the job was submitted. The repayment process after the end of the job is also performed for the point usage of the month in which the job was submitted.
    - The points usage of the Reserved service are counted in the month in which the reservation was made. If you cancel the reservation, it will be returned to the points used in the month you made the reservation.



## Checking Disk Quota

To display your disk/inodes usage and quota about home and group area, use the `show_quota` command.
The major option of the `show_quota` command are follows.

| Option | Description |
|:--|:--|
| -b *G* | Display the disk size in Gibibyte. By default, the display is in Tebibytes. |


Example) Display disk and inode quota

```
[username@login1 ~]$ show_quota
Disk quotas for ABCI group grpname
  Directory                          used(TiB)        limit(TiB)      used(nfiles)     limit(nfiles)
  /groups/grpname                            0                32            422372         200000000

Disk quotas for user username
  Directory                          used(TiB)        limit(TiB)      used(nfiles)     limit(nfiles)
  /home/username                             0                 2            103219                 0
```

| Item  | Description |
|:-|:-|
| Directory  | Assignment directory |
| used(TiB)  | Disk usage |
| limit(TiB) | Disk quota limit |
| used(nfiles) | Number of inodes |
| limit(nfiles) | inode quota limit |

Example) Display disk and inode quota in Gibibyte

```
[username@login1 ~]$ show_quota -b G
Disk quotas for ABCI group grpname
  Directory                          used(GiB)        limit(GiB)      used(nfiles)     limit(nfiles)
  /groups/grpname                          312             32768            422372         200000000

Disk quotas for user username
  Directory                          used(GiB)        limit(GiB)      used(nfiles)     limit(nfiles)
  /home/username                            32              2048            103221                 0
```

| Item  | Description |
|:-|:-|
| Directory  | Assignment directory |
| used(GiB)  | Disk usage |
| limit(GiB) | Disk quota limit |
| used(nfiles) | Number of inodes |
| limit(nfiles) | inode quota limit |

In case "0" is displayed in the culumn for the inodes quota limit, the number of inodes is unlimited.
If the disk usage exceeds the disk limit, the column displays "*".

When the number of inodes (disk usage) exceeds the inode (disk) quota limit, the file or directory creation is failed with the following message.

```
[username@login1 ~]$ touch quota_test
touch: cannot touch 'quota_test': Disk quota exceeded
```