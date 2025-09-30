# ABCI System Overview

## System Architecture

The ABCI system consists of 766 compute nodes with 6,128 NVIDIA H200 GPU accelerators and other computing resources, 75PB of physical storage, InfiniBand network that connects these elements at high speed, firewall, and so on. It also includes software to make the best use of these hardware. And, the ABCI system uses SINET6, the Science Information NETwork, to connect to the Internet at 400 Gbps.

## Computing Resources

Below is a list of the computational resources of the ABCI system.

| Node Type | Hostname | Description | # |
|:--|:--|:--|:--|
| Access Server | *as.v3.abci.ai* | SSH server for external access | 2 |
| Interactive Node | *login* | Login server, the frontend of the ABCI system | 5 |
| Compute Node (H) | *hnode001*-*hnode766* | Server w/ NVIDIA H200 GPU accelerators | 766 |

!!! note
    Due to operational and maintenance reasons, some computing resources may not be provided.

Among them, each interactive node and compute node (H) are equipped with InfiniBand HDR (200 Gbps) and are connected to Storage Systems described later by InfiniBand switch group.
Also, each compute node (H) is equipped with 8 port of InfiniBand NDR (200 Gbps) and the compute nodes (H) are connected by InfiniBand switch.

Below are the details of these nodes.

### Interactive Node

The interactive node of ABCI system consists of HPE ProLiant DL380 Gen11.
The interactive node is equipped with two Intel Xeon Platinum 8468 Processors and approximately 1024 GB of main memory available.

The specifications of the interactive node are shown below:

| Item| Description | # |
|:--|:--|:--|
| CPU | Intel Xeon Platinum 8468 Processor 2.1 GHz, 48 Cores | 2 |
| Memory | 64 GB DDR5-4800 | 16 |
| SSD | SAS SSD 960 GB | 2 |
| SSD | NVMe SSD 3.2 TB | 4 |
| Interconnect | InfiniBand HDR (200 Gbps) | 2 |
| | 10GBASE-SR | 1 |

Users can login to the interactive node, the frontend of the ABCI system, using SSH tunneling via the access server.

The interactive node allows users to interactively execute commands, and create and edit programs, submit jobs, and display job statuses. The interactive node does not have a GPU, but users can use it to develop programs for compute nodes (H).

Please refer to [Getting Started ABCI](getting-started.md) for details of login method and [Job Execution](job-execution.md) for details of job submission method.

!!! warning
    Do not run high-load tasks on the interactive node, because resources such as CPU and memory of the interactive node are shared by many users. If you want to perform high-load pre-processing and post-processing, please the compute nodes.
	Please note that if you run a high-load task on the interactive node, the system will forcibly terminate it.

### Compute Node

To execute the program for the compute node, submit the program to the job management system as a batch job or an interactive job. Interactive jobs allow you to compile and debug programs, and run interactive applications, visualization software and so on. For details, refer to [Job Execution](job-execution.md).

#### Compute Node (H)

The compute node (H) of ABCI system consists of HPE Cray XD670.
The compute node (H) is equipped with two Intel Xeon Platinum 8558 Processors and eight NVIDIA H200 GPU accelerators. In the entire system, the total number of CPU cores is 73,536 cores, and the total number of GPUs is 6,128.

The specifications of the compute node (H) are shown below:

| Item | Description | # |
|:--|:--|:--|
| CPU | Intel Xeon Platinum 8558 2.1GHz, 48cores | 2 |
| GPU | NVIDIA H200 SXM 141GB | 8 |
| Memory | 64 GB DDR5-5600 4400 MHz | 32 |
| SSD | NVMe SSD 7.68 TB | 2 |
| Interconnect | InfiniBand NDR (200 Gbps) | 8 |
| | InfiniBand HDR (200 Gbps) | 1 |
| | 10GBASE-SR | 1 |


## Storage Systems

The ABCI system has three storage systems for storing large amounts of data used for AI and Big Data applications, and these are used to provide shared file systems. Combined, /home, /groups, and the storage planned to be provided in the future<!--, and /groups_s3--> have an effective capacity of approximately 75 PB.

| # | Storage System | Media | Usage |
|:--|:--|:--|:--|
| 1 | DDN ES400NVX2 | 61.44TB NVMe SSD x256 | Home area(/home) |
| 2 | DDN ES400NVX2 | 61.44TB NVMe SSD x1280 | Group area(/groups) |
<!--| 3 | DDN ES400NVX2 | 30.72TB NVMe SSD x48 | ABCI Object area(/groups_s3) |-->

Below is a list of shared file systems provided by the ABCI system using the above storage systems.

| Usage | Mount point | Effective capacity | File system | Notes |
|:--|:--|:--|:--|:--|
| Home area | /home | 10 PB | Lustre |  |
| Group area | /groups | 63 PB | Lustre |  |
| ABCI Cloud Storage | /groups_s3 | 1PB | Lustre |  |
<!--| ABCI Object area | /groups_s3 | 1 PB | Lustre |  |-->

The following file systems are mounted for the purpose of data migration.

| Usage | Mount point | Effective capacity | File system | Notes |
|:--|:--|:--|:--|:--|
| Archive | /groups-2.0 | 10.8 PB | Lustre | Read-only. Group area used in ABCI 2.0 |


Interactive nodes, and compute nodes (H) mount the shared file systems, and users can access these file systems from common mount points.

Besides this, these nodes each have local storage that can be used as a local scratch area. The list is shown below.

| Node type | Mount point | Capacity | File system | Notes |
|:--|:--|:--|:--|:--|
| Interactive node | /local | 12 TB | XFS | |
| Compute node (H) | /local1 | 7 TB | XFS |  |
|                 | /local2 | 7 TB | XFS | Includes BeeGFS |

Storage Service Specifications:

| Storage | Initial Available Capacity | Capacity Limit | Number of Inode | Inode Limit | ABCI Points Consumption<br>(Standard Use) | ABCI Points Consumption<br>(Accelerated Development Use) |
| :- | :- | :- | :- | :- | :- | :- |
| /home | 2TB | 2TB | | | Free | Free |
| /groups | 0TB | 1000TB | 200 million | 600 million | 5 points per month per 1TB | 2.5 points per month per 1TB |
| /groups-2.0 | - | - | - | - | Free | Free |

!!! note
    * The storage service allocates 1,099,511,627,776 bytes per 1TB. 
    * The /home file system is shared by all users, and its total number of inodes is about 25 billion. There is no limit on the number of inodes in the /home directory for individual users.
    * The capacity of /groups can be increased up to 1,000TB via the user portal[^footnote01]. 
    * If you need more than 1,000TB of capacity or want to increase the inode limit in /groups, you need to apply via email. For application procedures, please refer to [Request for Increasing Group Area Quota](requests/group-area-quota-increase.md). 
    * The /groups-2.0 service will be discontinued at the end of September 2025. For more details, please refer to [Future Storage Service Announcements](https://abci.ai/news/2024/09/04/en_storage_after_nov2024.html).

[^footnote01]: The user portal is scheduled to be available from March 18, 2025.

## Software

The software available on the ABCI system is shown below.

| Category | Software | Interactive Node | Compute Node (H) |
|:--|:--|:--|:--|
| OS | Rocky Linux | - | 9.4 |
| OS | Red Hat Enterprise Linux | 9.4 | - |
| Job Scheduler | Altair PBS Professional | 2022.1.6 | 2022.1.6 |
| Development Environment | CUDA Toolkit | 11.8.0<br>12.0.1<br>12.1.1<br>12.2.2<br>12.3.2<br>12.4.1<br>12.5.1<br>12.6.1<br>12.8.1<br>12.9.1 | 11.8.0<br>12.0.1<br>12.1.1<br>12.2.2<br>12.3.2<br>12.4.1<br>12.5.1<br>12.6.1<br>12.8.1<br>12.9.1 |
| | Intel oneAPI<br>(compilers and libraries) | 2024.2.1 | 2024.2.1 |
| | GCC | 11.5.0<br>13.2.0 | 11.4.1<br>13.2.0 |
| | Python | 3.9.21<br>3.12.9<br>3.13.2 | 3.9.21<br>3.12.9<br>3.13.2 |
| | Ruby | 3.0.7 | 3.0.4 |
| | R | 4.5.1 | 4.5.1 |
| | Java | 17.0.16 | 17.0.16 |
| | Scala | 3.5.2 | 3.5.2 |
| | Perl | 5.32.1 | 5.32.1 |
| | Go | 1.24.6 | 1.24.4 |
| File System | DDN Lustre | 2.14.0_ddn196 | 2.14.0_ddn196 |
| | BeeOND | - | 7.4.5 |
| Object Storage | s3cmd | 2.4.0 | 2.4.0 |
| Container | SingularityCE | 4.1.5 | 4.1.5 |
| MPI | Intel MPI | 2021.13 | 2021.13 |
| | hpcx | 2.20 | 2.20 |
| | hpcx-debug | 2.20 | 2.20 |
| | hpcx-mt | 2.20 | 2.20 |
| | hpcx-prof | 2.20 | 2.20 |
| Library | cuDNN | 9.5.1<br>9.12.0 | 9.5.1<br>9.12.0 |
| | NCCL | 2.23.4-1<br>2.25.1-1 | 2.23.4-1<br>2.25.1-1 |
| | gdrcopy | 2.4.1 | 2.4.1 |
| | UCX | 1.17 | 1.17 |
| | Intel MKL | 2024.2.1 | 2024.2.1 |
| Utility | aws-cli | 1.29.62 | 1.29.62 |
| | s3fs-fuse | 1.94 | 1.94 |
| | rclone | 1.70.3 | 1.70.3 |

