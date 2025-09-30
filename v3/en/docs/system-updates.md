# System Updates

## 2025-08-26 {#2025-08-26}

We added the following softwares.

* CUDA Toolkit 12.9.1
* cuDNN 9.12.0
* firefox (compute nodes)

We granted the users access to NVIDIA GPU Performance Counters.

## 2025-08-08 {#2025-08-08}

We installed GCC 13.2.0.

## 2025-08-05 {#2025-08-05}

We have added a description for ABCI Cloud Storage to the manual.

\* The open day of the service for the users is expected to be in late August. Please note that ABCI Cloud Storage is only available within ABCI (login nodes and compute nodes) and is not currently available from external sources (such as the Internet). We will provide further information on external use as soon as it becomes available.

## 2025-07-25 {#2025-07-25}

Disabled the usage of qalter command by the users.

## 2025-07-11 {#2025-07-11}

We installed `qgstat_l` which is the lightweight version of qgstat.

## 2025-06-20 {#2025-06-20}

We have added the following applications as Open OnDemand Interactive Apps.

* code-server
* Interactive Desktop(Xfce)

## 2025-04-15 {#2025-04-15}

We implemented the following features:

* SSH access control option when submitting jobs
    * With the addition of the `qsub` command option, you can now control SSH login to compute nodes while a job is running. For detailed, please refer to [Job Execution Options](job-execution.md#job-execution-options).
* Open OnDemand Job Composer

The following software has been updated:

| Add / Update / Delete | Software | Version | Previous version |
|:--|:--|:--|:--|
| Update | Lustre-client | 2.14.0_ddn196 | 2.14.0_ddn172 |
| Update | python (Interactive Node) | 3.9.21 | 3.9.18 |
| Add | python | 3.12.9 | |
| Add | python | 3.13.2 | |

The ABCI 2.0 home area (`/home-2.0`) that was mounted for data migration has been unmounted due to the end of provision.

## 2025-03-03 {#2025-03-03}

SingularityPRO is now available. For detailed usage instructions, please refer to [How to use SingularityPRO](containers.md#how-to-use-singularitypro).

## 2025-01-31 {#2025-01-31}

With the addition of the `qrstat --available=grpname` feature, you can now check the availability of node reservations. For more details on how to use this feature, please refer to [show the status of reservations](job-execution.md#show-the-status-of-reservations).
