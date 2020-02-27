# File Server Protocol Family Test Suite User Guide

## Table of Contents

* [1 Introduction](#1)
  * [1.1 Testing Environments](#1.1)
* [2 Test Suite Setup Overview](#2)
  * [2.1 Installed Files and Folders](#2.1)
* [3 Requirements](#3)
  * [3.1 Network Infrastructure](#3.1)
  * [3.2 Environment Configuration](#3.2)
  * [3.3 Hardware](#3.3)
    * [3.3.1 Driver Computer Minimum Requirements](#3.3.1)
    * [3.3.2 System Under Test (SUT) Minimum Requirements](#3.3.2)
    * [3.3.3 Domain Controller (DC) Minimum Requirements](#3.3.3)
    * [3.3.4 SAN Storage Server Minimum Requirements](#3.3.4)
  * [3.4 Software](#3.4)
* [4 Network Setup](#4)
  * [4.1 Workgroup Network Environment](#4.1)
    * [4.1.1 Workgroup Message Architecture](#4.1.1)
    * [4.1.2 Workgroup Network Architecture](#4.1.2)
  * [4.2 Domain Network Environment](#4.2)
    * [4.2.1 Domain Message Architecture](#4.2.1)
    * [4.2.2 Domain Network Architecture](#4.2.2)
  * [4.3 Verify Connectivity from the Driver Computer](#4.3)
* [5 Computer Setup](#5)
  * [5.1 Workgroup Environment](#5.1)
    * [5.1.1 Set up the Driver Computer for the Workgroup Environment](#5.1.1)
    * [5.1.2 Set up the SUT Computer in the Workgroup Environment](#5.1.2)
  * [5.2 Domain Environment](#5.2)
    * [5.2.1 Set up the Domain Controller (DC) for the Domain Environment](#5.2.1)
    * [5.2.2 Set up the Driver Computer for the Domain Environment](#5.2.2)
    * [5.2.3 Set up the SAN Storage Server in the Domain Environment](#5.2.3)
    * [5.2.4 Set up the SUT in the Domain Environment](#5.2.4)
  * [5.3 General Setup Details for Windows Platforms](#5.3)
    * [5.3.1 Create a share](#5.3.1)
    * [5.3.2 Set NTFS Permissions](#5.3.2)
    * [5.3.3 Set Share Permissions](#5.3.3)
    * [5.3.4 Add a share SameWithSMBBasic with the same Path of Share SMBBasic](#5.3.4)
    * [5.3.5 Create symbolic links](#5.3.5)
    * [5.3.6 Enable Encrypt Data Access on a Share Named SMBEncrypted](#5.3.6)
    * [5.3.7 Enable FORCE_LEVELII_OPLOCK on a Share Named ShareForceLevel2](#5.3.7)
    * [5.3.8 Create a Share on a Volume Supporting Integrity](#5.3.8)
    * [5.3.9 Set up a Windows-based Domain Controller](#5.3.9)
    * [5.3.10 Set up the Distributed File System (DFS)](#5.3.10)
    * [5.3.11 Set up the SAN Storage Server](#5.3.11)
    * [5.3.12 Set up a Cluster](#5.3.12)
    * [5.3.13 Turn off Firewalls](#5.3.13)
    * [5.3.14 Enable the Local Administrator Account](#5.3.14)
    * [5.3.15 Create a Local Non-Admin User Account](#5.3.15)
    * [5.3.16 Enable the Local Guest Account](#5.3.16)
    * [5.3.17 Reset the Password for Local Users](#5.3.17)
    * [5.3.18 Create an Asymmetric Share](#5.3.18)
    * [5.3.19 Create a Virtual Hard Disk File](#5.3.19)
    * [5.3.20 Create a Shared Virtual Hard Disk File](#5.3.20)
    * [5.3.21 Modify the Signing Configuration](#5.3.21)
    * [5.3.22 Manually Set the Domain Account Computer Password](#5.3.22)
    * [5.3.23 Create an SQOS Policy](#5.3.23)
    * [5.3.24 How to Start the Command Console with Administrator Privileges](#5.3.24)
    * [5.3.25 Enable Short Name format](#5.3.25)
    * [5.3.26 Create a Volume Shadow Copy](#5.3.26)
    * [5.3.27 Create a Volume Mount Point](#5.3.27)
* [6 Installed Files and Folders](#6)
* [7 Configure and Run Test Cases](#7)
  * [7.1 Configure and Run Test Cases Using Protocol Test Manager](#7.1)
  * [7.2 Configure Test Suite Manually](#7.2)
  * [7.3 Run Test Cases with Batch Script](#7.3)
    * [7.3.1 Run the BVT Test](#7.3.1)
    * [7.3.2 Run All Test Cases](#7.3.2)
  * [7.4 Reviewing Test Results](#7.4)
* [8 Creating a Custom Test Environment](#8)
* [9 Debugging Test cases](#9)

## <a name="1"/> 1 Introduction

This user guide provides the fundamentals for using the **File Server Protocol Family Test Suite**. In this guide, you will learn how to do the following:

* Install the test suite.
* Configure the network for the test environment.
* Set up the Driver computer, system under test (SUT), and other computers.
* Set up the test configuration.
* Run the test cases

The **File Server Protocol Family Test Suite**, hereinafter referred to as the **Test Suite**, is designed to test implementations of the File Server protocol family, which includes [the following protocols that are documented on the Microsoft Technical Documents site](https://msdn.microsoft.com/en-us/library/jj712081.aspx):

* [**[MS-SMB2]**](https://msdn.microsoft.com/en-us/library/cc246482.aspx) (Server Message Block (SMB) Protocol Version 2 and 3)
* [**[MS-FSRVP]**](https://msdn.microsoft.com/en-us/library/hh554852.aspx) (File Server Remote VSS Protocol)
* [**[MS-SWN]**](https://msdn.microsoft.com/en-us/library/hh536748.aspx) (Service Witness Protocol)
* [**[MS-DFSC]**](https://msdn.microsoft.com/en-us/library/cc226982.aspx) Distributed File System (DFS): Namespace Referral Protocol
* [**[MS-SQOS]**](https://msdn.microsoft.com/en-us/library/mt226249.aspx) (Storage Quality of Service Protocol)
* [**[MS-RSVD]**](https://msdn.microsoft.com/en-us/library/dn393384.aspx) (Remote Shared Virtual Disk Protocol
* [**[MS-FSA]**](https://msdn.microsoft.com/en-us/library/ff469524.aspx) (File System Algorithms)

This guide specifies the details for using the **Test Suite**  with both Microsoft® Windows® operating system and non-Windows based operating system environments. However, the details about non-Windows systems are limited to generalities, as the focus of setup and configuration is on Windows systems in this User Guide.

### <a name="1.1"/> 1.1 Testing Environments

You can use the **Test Suite** to perform test cases against your implementation in either a **WORKGROUP** or **DOMAIN** environment. You might choose the **WORKGROUP** environment if you are new to the protocol test suites or if you want to simplify the setup and environment in which to perform test cases. On the other hand, you might choose the **DOMAIN** environment if you want to run test cases against your implementation in a more comprehensive environment that more closely resembles a real world environment containing a Domain Controller, File Server, Storage Server, and Clustering.

Whichever environment you choose, the details in this document specify the steps you should take to set up the chosen environment in which you plan to run your tests. For example, if you choose the **WORKGROUP** environment, you would simply ignore the steps that apply to the **DOMAIN** environment and vice-versa.

![](./image/FileServerUserGuide/image1.png)**Note**

The **Test Suite** examines protocol implementation behaviors that are observed over the wire only.

**More Information**

To learn more about File Server Test Suite design, see the following documentation:
  
* [File Server Protocol Family Test Design Specification](~/blob/staging/TestSuites/FileServer/docs/FileServerTestDesignSpecification.md)
* [Authentication Protocol Server Test Design Specification](~/blob/staging/TestSuites/FileServer/docs/Auth_ServerTestDesignSpecification.md)
* [MS-FSA Protocol Server Test Design Specification](~/blob/staging/TestSuites/FileServer/docs/MS-FSA_ServerTestDesignSpecification.md)
* [MS-SQOS Protocol Server Test Design Specification](~/blob/staging/TestSuites/FileServer/docs/MS-SQOS_ServerTestDesignSpecification.md)

![](./image/FileServerUserGuide/image1.png)**Tip**

To learn more about the common environment in which test cases are run for all **Test Suites**, see the [Protocol Test Framework User Guide](https://github.com/microsoft/ProtocolTestFramework/blob/staging/docs/PTFUserGuide.md).

## <a name="2"/> 2 Test Suite Setup Overview

The following table summarizes the tasks that are required to get the **File Server Test Suite** up and running. Each task in the list also provides one or more links to related topics in this User Guide that you can review prior to actually performing the tasks. After you review (or complete) each task, you can optionally print out Table 1 and check off completed items.

<a name="table.1"></a>

**Table 1. Overview of Test Suite setup tasks**

| Check | Task Overview                                                                                                       | Related Topic                                                                                                                                                                          |
|-------|---------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| □     | Download the Test Suite installer file **FileServer-TestSuite-ServerEP.msi** here.                                  | For a complete list of files contained in the Test Suite installer, please see section [6 Installed Files and Folders](#6).                                                            |
| □     | Plan your test environment and computer configurations with respect to the **File Server Test Suite** requirements. | To review the network, environment, and hardware requirements for the **Test Suite**, see section [3 Requirements](#3).                                                                |
| □     | Install the **File Server Test Suite** and supporting software on what will be your Driver computer.                | To review the complete list of software that must be sequentially installed, which includes the **Test Suite** and supporting software, see section [3.4 Software Requirements](#3.4). |
| □     | Set up the network.                                                                                                 | To review details, see section [4 Network Setup](#4).                                                                                                                                  |
| □     | Verify connectivity between the Driver computer, SUT, and other network computers, as necessary.                    | To review details, see section [4.3 Verifying Network Computer Connectivity](#4.3).                                                                                                    |
| □     | Set up a Workgroup environment.                                                                                     | To review details, see section [5.1 Workgroup Environment](#5.1).                                                                                                                      |
| □     | Set up a Domain environment.                                                                                        | To review details, see section [5.2 Domain Environment](#5.2).                                                                                                                         |
| □     | Configure the **Test Suite** settings.                                                                              | To review the details about configuring your Test Suite settings, see section [7 Configure and Run Test Cases Using Protocol Test Manager](#7).                                        |
| □     | Run build verification tests (BVT).                                                                                 | To review details, see section [7.3.1 Run the BVT Tests](#7.3.1).                                                                                                                      |
| □     | Run test cases.                                                                                                     | To review details, see section [7.3.2 Run All Test Cases](#7.3.2).                                                                                                                     |
| □     | Assess the results of test cases you have run.                                                                      | To review details, see section [7.4 Reviewing Test Results](#7.4).                                                                                                                     |
| □     | Create your own test cases.                                                                                         | For further details, see section [8 Creating a Custom Test Environment](#8).                                                                                                           |
| □     | Debug your test cases.                                                                                              | For further details, see section [9 Debugging Test Cases](#9).                                                                                                                         |

## <a name="3"/> 3 Requirements

This section specifies what is necessary to set up the test environment, which includes requirements for the following:
  
* [Network Infrastructure](#3.1)
* [Environment Configuration](#3.2)
* [Hardware](#3.3)
* [Software](#3.4)

### <a name="3.1"/> 3.1 Network Infrastructure

The network infrastructure for the Test Suite requires the following:

* A test network that connects the test computer systems and has the following characteristics:
  * It must utilize an isolated hub or switch.
  * It must not be connected to a production network nor should it be used for any other business or personal operations.
  * It must not be connected to the internet.
* IP addresses that you will assign for this test network.
* Computer names that you will assign in the test network.
* User credentials that are dedicated to the test network components.

### <a name="3.2"/> 3.2 Environment Configuration

You can execute the Test Suite test cases in either a **WORKGROUP** or **DOMAIN** environment.

* To run the **Test Suite** in a **WORKGROUP** environment, the test machines in the list that follows are required (either physical or virtual). See [Figure 1: Workgroup environment network configuration](#fig.1) for reference.

  ![](./image/FileServerUserGuide/image1.png)**Note**
  
  Although you can install and run the **Test Suite** components on virtual machines, this User Guide describes the setup of physical machines only.

  * **Driver computer** (Client01) − this machine must be running the Microsoft® Windows 8.1 Enterprise operating system or later.

    See section [3.3.1 Driver Computer Minimum Requirements](#3.3.1) for hardware requirements and section [5.1.1 Set up the Driver Computer in the Workgroup Environment](#5.1.1) for setup instructions.

  * **SUT computer** (Node01) − this machine can be for hosting either a Windows or non-Windows implementation. For a Windows implementation, the SUT computer must be running the Microsoft® Windows Server 2012 R2, Standard Edition or later operating system.

    See section [3.3.2 System Under Test (SUT) Minimum Requirements](#3.3.2) for hardware requirements and section [5.1.2 Setup the SUT Computer in the Workgroup Environment](#5.1.2) for setup instructions.

* To run the Test Suite in a DOMAIN environment, the test machines in the list that follows are required (either physical or virtual). See [Figure 2: Domain environment network configuration](#fig.2) for reference.

  * **Domain Controller** (DC01) – this machine can be running either a Windows or non-Windows operating system.

    ![](./image/FileServerUserGuide/image1.png)**Note**

    If you choose to set up a non-Windows computer, you do so at your own discretion.

    See section [3.3.3 Domain Controller (DC) Minimum Requirements](#3.3.3) for hardware requirements and section [5.2.1 Set up the Domain Controller for the Domain Environment](#5.2.1) for setup instructions.

  * **Driver computer** (Client01) − this machine must be running the Microsoft® Windows 8.1 Enterprise operating system or later.

    See section [3.3.1 Driver Computer Minimum Requirements](#3.3.1) for hardware requirements and section [5.2.2 Set up the Driver Computer for the Domain Environment](#5.2.2) for setup instructions.

  * **Clustering** − if you want to test cluster scenarios, you will need the following machines:

    * **Storage Area Network (SAN) server** (Storage01) − as part of the SUT configuration, this machine can be for testing either a Windows or non-Windows implementation.

      See section [3.3.4 SAN Storage Server](#3.3.4) for hardware requirements and section [5.2.3 Set up the SAN Storage Server](#5.2.3) for setup instructions.

    * **Failover cluster computers** (Node01, Node02) − as part of the SUT configuration, these machines can be for testing either a Windows or non-Windows implementation. Note that 2 SUT computers are required if you are deploying a Windows implementation.

      See section [3.3.2 System Under Test (SUT) Minimum Requirements](#3.2.2) for hardware requirements and section [5.2.4 Set up the SUT for the Domain Environment](#5.2.4) for setup instructions.

  * **Non Clustering** − if you are not testing clustering scenarios, you will need the following:

    * **SUT computer** − this machine can be for testing either a Windows or non-Windows implementation.

      ![](./image/FileServerUserGuide/image1.png)**Note**

      If you want to adjust an existing environment for testing non-clustering scenarios, you can simplify your deployment steps by using a node in an existing Failover Cluster configuration as the SUT computer.

**More Information**

To learn more about Failover Cluster scenarios, see the **Test Suite Design** section in the [File Server Protocol Family Test Design Specification](https://github.com/Microsoft/WindowsProtocolTestSuites/blob/staging/TestSuites/FileServer/docs/FileServerTestDesignSpecification.md).

### <a name="3.3"/> 3.3 Hardware

Hardware requirements for the test computers in the **WORKGROUP** and **DOMAIN** environments are described in this section.

#### <a name="3.3.1"/> 3.3.1 Driver Computer Minimum Requirements

The minimum requirements for the Driver computer are specified in Table 2 that follows.

![](./image/FileServerUserGuide/image1.png)**Note**

The requirements that follow apply only to a Windows-based Driver computer in the **WORKGROUP** or **DOMAIN** environments.

<a name="table.2"></a>

**Table 2. Driver computer minimum requirements in WORKGROUP or DOMAIN environment**

| Requirement       | Description                                                  |
|-------------------|--------------------------------------------------------------|
| Computer name     | Client01                                                     |
| Operating system  | Microsoft® Windows 8.1, Enterprise Edition or later versions |
| Memory            | 2 GB RAM                                                     |
| Disk space        | 60 GB                                                        |
| Network interface | Requires dual network interface cards (NICs)                 |

#### <a name="3.3.2"/> 3.3.2 System Under Test (SUT) Minimum Requirements

The minimum requirements for the SUT computer in the Windows environment are specified herein. Table 3 below specifies the requirements that apply to the SUT computer in the **WORKGROUP** environment. Table 4 specifies the requirements that apply to both SUT computers for a Windows implementation that uses a File Server, Storage, and Failover clustering configuration in the **DOMAIN** environment.  

![](./image/FileServerUserGuide/image1.png)**Note**

The requirements that follow apply only to Windows-based SUT computers in the test environment.

<a name="table.3"></a>

**Table 3. SUT computer minimum requirements in the WORKGROUP environment**

| Requirement       | Description                                                                                                                               |
|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| Computer name     | Node01                                                                                                                                    |
| Operating system  | Microsoft® Windows Server 2012 R2, Standard Edition or later versions. Note that this computer is **not** set up as a domain controller (DC). |
| Features          | N/A                                                                                                                                       |
| Role Services     | File Server                                                                                                                               |
| Memory            | 2 GB RAM                                                                                                                                  |
| Disk space        | 60 GB                                                                                                                                     |
| Network interface | Requires dual network interface cards (NICs)                                                                                              |

<a name="table.4"></a>

**Table 4. SUT computer minimum requirements in the DOMAIN environment**

| Requirement       | Description                                                                              |
|-------------------|------------------------------------------------------------------------------------------|
| Computer name     | Node01,Node02                                                                            |
| Operating system  | Microsoft® Windows Server 2012 R2, Standard Edition or later versions.                   |
| Features          | Failover Clustering                                                                      |
| Role Services     | File Server, DFS Namespaces, File Server Resource Manager, File Server VSS Agent Service |
| Memory            | 2 GB RAM                                                                                 |
| Disk space        | 60 GB                                                                                    |
| Network interface | Requires dual network interface cards (NICs)                                             |

#### <a name="3.3.3"/> 3.3.3 Domain Controller (DC) Minimum Requirements

The minimum requirements for the Domain Controller computer are specified in Table 5 that follows.

![](./image/FileServerUserGuide/image1.png)**Note**

The requirements that follow apply only to a Windows-based DC in the test environment.

<a name="table.5"></a>

**Table 5. Domain Controller computer minimum requirements in the DOMAIN environment**

| Requirement       | Description                                                            |
|-------------------|------------------------------------------------------------------------|
| Computer name     | DC01                                                                   |
| Operating system  | Microsoft® Windows Server 2012 R2, Standard Edition or later versions. |
| Features          | AD DS Tools, DNS Server Tools                                          |
| Role Services     | Active Directory Domain Services (AD DS), DNS Server                   |
| Memory            | 1 GB RAM                                                               |
| Disk space        | 60 GB                                                                  |
| Network interface | Requires dual network interface cards (NICs)                           |

#### <a name="3.3.4"/> 3.3.4 SAN Storage Server Minimum Requirements

The minimum requirements for the storage area network (SAN) server computer are as follows.

![](./image/FileServerUserGuide/image1.png)**Note**

The requirements that follow apply only to a Windows-based SAN server in the test environment.

<a name="table.6"></a>

**Table 6. SAN server computer minimum requirements in DOMAIN environment**

| Requirement      | Description                                                            |
|------------------|------------------------------------------------------------------------|
| Computer name    | Storage01                                                              |
| Operating system | Microsoft® Windows Server 2012 R2, Standard Edition or later versions. |
| Features         | N/A                                                                    |
| Role Services    | File Server, iSCSI Target Server                                       |
| Memory           | 1 GB RAM                                                               |
| Disk space       | 60 GB                                                                  |

### <a name="3.4"/> 3.4 Software

All of the following software must be installed on the driver computer. They have some dependencies on each other, it is recommended to install them in the order listed below so that they can work correctly.

**Required Software**

All common softwares listed in [prerequisites](https://github.com/microsoft/WindowsProtocolTestSuites#prerequisites) for running Windows Protocol Test Suites.

**Optional Software**

* **Protocol Test Manager**

    **Protocol Test Manager** provides a graphical user interface (UI) to facilitate configuration and execution of Microsoft® Windows Protocol Test Suite tests. Its use is highly recommended.

* **Microsoft Message Analyzer**

    **Microsoft® Message Analyzer** (MMA) is listed here as an optional tool because the test cases of themselves neither perform live captures or capture verifications during execution. However, MMA can be helpful with debugging test case results, by analyzing ETL files that are generated by the Test Cases, that is, if you enable the the Automatic Network Capturing feature in the Protocol Test Manager (PTM) during test case configuration. The Automatic Network Capturing feature is further described in the [PTF User Guide](https://github.com/Microsoft/ProtocolTestFramework/blob/staging/docs/PTFUserGuide.md#-automatic-network-capturing).

    ![](./image/FileServerUserGuide/image1.png)Note

    November 25 2019 - Microsoft Message Analyzer (MMA) has been retired and removed from public-facing sites on microsoft.com. A private MMA build is available for testing purposes; to request it, send an email to [getmma@microsoft.com](mailto:getmma@microsoft.com).

## <a name="4"/> 4 Network Setup

This section describes how to set up the network for both the **WORKGROUP** and **DOMAIN** environments, which are described earlier in section [3.2 Environment Configuration](#3.2).

![](./image/FileServerUserGuide/image1.png)**Note**

You can deploy the **Test Suite** on either physical or virtual machines. However, the sections that follow describe the deployment on physical machines only.

**More Information**

To learn more about how to configure a virtual machine, see
[Create Virtual Machine with Hyper-V on Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/create-virtual-machine)

### <a name="4.1"/> 4.1 Workgroup Network Environment

#### <a name="4.1.1"/> 4.1.1 Workgroup Message Architecture

Computers in the **WORKGROUP** network environment interact with each other as follows:

* The Driver computer (Client01) runs test cases which generate  requests that are sent over the wire as protocol messages to the SUT (Node01) computer.

* The SUT computer responds to the requests from the Driver computer by running an implementation of the test protocol, which returns a response that is evaluated by a particular test case.

#### <a name="4.1.2"/> 4.1.2 Workgroup Network Architecture

The following diagram shows the **WORKGROUP** environment with recommended network configurations for reference. Note that all the settings are configurable.

![](./image/FileServerUserGuide/image1.png)**Note**

The suggested settings in the following diagram apply to both Windows-based computers and non-Windows based computers in a **WORKGROUP** environment, however, note that this User Guide documents the Windows case only.

<a name="fig.1"></a>

![](./image/FileServerUserGuide/image3.png)

Figure 1. Workgroup environment network configuration

![](./image/FileServerUserGuide/image1.png)**Note**

As a non-Windows SUT computer, **Node01** should host an implementation-specific system under test.

<a name="table.7"></a>

**Table 7. Suggested machine names and network configuration for the WORKGROUP environment**

| Machine Name/Access Point | NIC       | IPv4          | Subnet Mask   |
|---------------------------|-----------|---------------|---------------|
| Client01                  | External1 | 192.168.1.111 | 255.255.255.0 |
|                           | External2 | 192.168.1.112 | 255.255.255.0 |
| Node01                    | External1 | 192.168.1.11  | 255.255.255.0 |
|                           | External2 | 192.168.1.12  | 255.255.255.0 |

![](./image/FileServerUserGuide/image1.png)**Note**

The addresses that are specified in the IPv4 column of this table and other tables are placeholders for actual addresses that you should specify. Also, note that the Driver computer (Client01) and the SUT computer (Node01) each require dual network interface cards (NICs), as indicated by the "External1" and "External2" designations, for complete testing of all scenarios.

### <a name="4.2"/> 4.2 Domain Network Environment

#### <a name="4.2.1"/> 4.2.1 Domain Message Architecture

Computers in the **DOMAIN** environment interact with each other as follows:

* The Driver computer runs test cases which generate requests that are sent over the wire as protocol messages to one or more SUT computers, for example, computers comprising a Failover Cluster configuration.

* The SUT/s respond/s to requests from the Driver computer by running an implementation of the protocol which returns a reply that is evaluated by a particular test case.

* The domain controller (DC) responds to security authentication requests such as logging in, checking permissions, and so on, within the domain.

* Other interactions include when the Driver computer runs test cases to generate messages over the wire that run protocol implementations for testing the functions and capabilities of the storage area network (SAN) backend storage for the Failover Cluster.

#### <a name="4.2.2"/> 4.2.2 Domain Network Architecture

The following diagram shows the **DOMAIN** environment with recommended network configurations for reference. Note that all settings are configurable.

![](./image/FileServerUserGuide/image1.png)**Note**

The suggested settings in the following diagram apply to Windows based computers in the DOMAIN environment only.  However, non-Windows based computers are also supported in this environment. Note that dual network interface cards (NICs) are required on the Driver, SUT, and DC computers, as indicated by the "External1" and "External2" designations.

In Clustering scenarios, the Driver computer (Client01 in figure 2) contacts the **Node01** and **Node02** SUT computers, which are configured as Failover Cluster nodes. The Storage Area Network (SAN) server computer **Storage01** is on an isolated network and provides backend storage for the Failover Cluster.

In non-clustering scenarios as well, the Driver computer contacts whichever node is acting as the SUT computer.

<a name="fig.2"></a>

![](./image/FileServerUserGuide/image4.png)

Figure 2. Domain environment network configuration

![](./image/FileServerUserGuide/image1.png)**Note**

In Figure 2, **Cluster01** represents the Failover Cluster computers that are identified as **Node01** and **Node02**, while **GeneralFS** represents the File Server role provided by the Failover Cluster, and **ScaleOutFS** represents the Scale-Out File Server role provided by the same cluster.

In non-Windows environments, the **Node01**, **Node02**, and **Storage01** computers could be replaced by  some other implementation-specific system under test (SUT).

<a name="table.8"></a>

**Table 8. Suggested machine names and network configuration for the DOMAIN environment**

| Machine Name/EndPoint | NIC       | IPv4          | Subnet Mask   | Default Gateway | DNS Server    |
|-----------------------|-----------|---------------|---------------|-----------------|---------------|
| DC01                  | External1 | 192.168.1.1   | 255.255.255.0 | &lt;empty&gt;   | 127.0.0.1     |
|                       | External2 | 192.168.2.1   | 255.255.255.0 | &lt;empty&gt;   | 127.0.0.1     |
| Client01              | External1 | 192.168.1.111 | 255.255.255.0 | 192.168.1.1     | 192.168.1.1   |
|                       | External2 | 192.168.2.111 | 255.255.255.0 | 192.168.2.1     | 192.168.2.1   |
| Node01                | External1 | 192.168.1.11  | 255.255.255.0 | 192.168.1.1     | 192.168.1.1   |
|                       | External2 | 192.168.2.11  | 255.255.255.0 | 192.168.2.1     | 192.168.2.1   |
|                       | Cluster   | 10.10.0.11    | 255.255.255.0 | &lt;empty&gt;   | &lt;empty&gt; |
|                       | iSCSI     | 192.168.0.11  | 255.255.255.0 | &lt;empty&gt;   | &lt;empty&gt; |
| Node02                | External1 | 192.168.1.12  | 255.255.255.0 | 192.168.1.1     | 192.168.1.1   |
|                       | External2 | 192.168.2.12  | 255.255.255.0 | 192.168.2.1     | 192.168.2.1   |
|                       | Cluster   | 10.10.0.12    | 255.255.255.0 | &lt;empty&gt;   | &lt;empty&gt; |
|                       | iSCSI     | 192.168.0.12  | 255.255.255.0 | &lt;empty&gt;   | &lt;empty&gt; |
| Storage01             | iSCSI     | 192.168.0.1   | 255.255.255.0 | &lt;empty&gt;   | &lt;empty&gt; |
| Cluster01             | External1 | 192.168.1.100 |               |                 |               |
|                       | External2 | 192.168.2.100 |               |                 |               |
| GeneralFS             | External1 | 192.168.1.200 |               |                 |               |
|                       | External2 | 192.168.2.200 |               |                 |               |

### <a name="4.3"/> 4.3 Verify Network Computer Connectivity

After setting up the host computers and the network configurations in either the **WORKGROUP** or **DOMAIN** environment, perform the following steps to test connectivity between Windows-based computers.

**More Information**

To learn more about testing computer connectivity, see the Administrative Guide for your operating system.

![](./image/FileServerUserGuide/image1.png)**Note**

Before performing the steps that follow, disable all active firewalls in the test environment . For Windows computers, see section [5.3.13 Turn off Firewalls](#5.3.13).

To check connectivity from the Driver computer to the SUT computer/s as well as other computers in your test environment, perform the following steps:

  1. Click the **Start** button on the Driver computer **Client01** and then click **Run**.

  2. In the **Run** dialog box, type `cmd` and then click **OK**.

  3. At the command prompt on the Driver computer, type the command `ping` followed by a space and then type the hostname or IP address of the SUT **Node01** computer, for example:

  ```
      ping Node01
  ```

  4. Press the **Enter** key on the Driver computer keyboard and verify that you receive 4 ping responses at the command line from the **pinged** computer.

  5. Repeat these steps for other computers in your test environment to confirm that there are no network connectivity issues.

![](./image/FileServerUserGuide/image1.png)**Caution**

Do not proceed further with the setup until you have verified the proper connectivity of all machines in your environment, according to the previous procedure. Otherwise, network connectivity issues may block  further configuration in the steps that follow.

## <a name="5"/> 5 Computer Setup

As indicated earlier, the Test Suite can be executed in either the **WORKGROUP** or **DOMAIN** environment, as described in section [3.2 Environment Configuration](#3.2).

![](./image/FileServerUserGuide/image2.png)**Important**

When setting up the Driver and SUT computers in either the **WORKGROUP** or **DOMAIN** environment, the procedures specify the use of particular names for items such as passwords, sharenames, and so on. Note that in most cases these are only suggested names that you can change to accommodate your specific environment, at your discretion.

### <a name="5.1"/> 5.1 Workgroup Environment

The **WORKGROUP** environment consists of two test machines as follows:

* **Driver computer** – must be running the Microsoft® Windows 8.1 Enterprise or later operating system version.

* **SUT computer** – must be running the Microsoft® Windows Server 2012 R2, Standard Edition or later version operating system for implementations in the Windows environment; in non-Windows environments, the operating system running on the SUT computer is at your own discretion, as is the implementation being tested on that computer.

#### <a name="5.1.1"/> 5.1.1 Set up the Driver Computer for the Workgroup Environment

This section describes how to set up the Driver computer for the **WORKGROUP** environment.

![](./image/FileServerUserGuide/image2.png)**Important**

Microsoft® Visual Studio, the Protocol Test Framework, and Spec Explorer must be installed on the Driver computer **before** you run the **Test Suite** installer. See section [3.4 Software Requirements](#3.4).

Perform the steps that follow to set up the Driver computer:

1. Configure IP addresses along with subnet masks and set the computer name to **Client01**. See section [4.1 Workgroup Environment](#4.1).

2. Make sure all firewall are turned off.

![](./image/FileServerUserGuide/image1.png)**Note**

For more information about turning off the firewall on Windows platforms, see section [5.3.13 Turn off Firewalls](#5.3.13).

3. Install the required software, as described in section [3.4 Software Requirements](#3.4), which includes installing the **FileServer-TestSuite-ServerEP.msi** package onto the Driver computer.

4. For x64-based machines, start Windows PowerShell (x64) with Administrator privileges by right-clicking the Windows **Start** menu and selecting **Windows Power Shell (Admin)**; then type following command:  

  ```
    Set-ExecutionPolicy Unrestricted
  ```

5. For x86-based machines, start Windows PowerShell (x86) with Administrator privileges by right-clicking the Windows **Start** menu and selecting **Windows Power Shell (Admin)**; then execute the same command as immediately above.

6. Create a new folder with path **%SystemDrive%\\FileServerCaptureFileDirectory**. This folder can be used to save the event trace log (ETL) files automatically generated by the Test Cases, which files you can analyze with Microsoft® Message Analyzer. Note that generation of ETL files will occur only if you enable **NetworkCapture** on the **Configure Test Cases** tab of the PTM and that you run the PTM as Administrator.

#### <a name="5.1.2"/> 5.1.2 Setup the SUT Computer in the Workgroup Environment

This section describes several ways to set up the SUT computer, according to the subsections that  follow:

* [Common Setup](#5.1.2.1)(section 5.1.2.1)
* [Setup to test SMB2](#5.1.2.2)(section 5.1.2.2)
* [Setup to test DFSC](#5.1.2.3)(section 5.1.2.3)
* [Setup to test RSVD](#5.1.2.4)(section 5.1.2.4)
* [Setup to test SQOS](#5.1.2.5)(section 5.1.2.5)
* [Setup to test FSA](#5.1.2.6)(section 5.1.2.6)

##### <a name="5.1.2.1"/> 5.1.2.1 Common Setup

Perform the following steps to create a common setup for the SUT computer in a Windows **WORKGROUP** environment:

1. Configure IP addresses on the SUT computer and set the computer name to **Node01**, as described in section [4.1 Workgroup Environment](#4.1).

2. Turn off the SUT computer firewall.

    For Windows platforms, see section [5.3.13 Turn off Firewalls](#5.3.13).

3. Ensure that the local **Administrator** account is enabled (which should be enabled by default) and create a local non-Administrative account named **nonadmin** for use with some test cases.

    For Windows platforms, see the following sections:

    [5.3.14 Enable the Local Administrator Account](#5.3.14)

    [5.3.15 Create a Local Non-Admin User Account](#5.3.15)

4. Ensure that the password for the local **Administrator** and **nonadmin** accounts are set to **Password01!**.

    See section [5.3.17 Reset Password for Local Users](#5.3.17).

5. Enable the Windows **Guest** account as described in section [5.3.16 Enable the Local Guest Account](#5.3.16). The Guest account is used in some test cases.

6. Reset the password for the **Guest** account to **Password01!**.

    See section [5.3.17 Reset Password for Local Users](#5.3.17).

In sections [5.1.2.2 Setup to test SMB2](#5.1.2.2) through [5.1.2.5 Setup to test SQOS](#5.1.2.5) that follow, you will create the configurations necessary for testing the Server Message Block (SMB) and other protocols.

##### <a name="5.1.2.2"/> 5.1.2.2 Setup to test SMB2

The procedures in the subsections that follow create the test configurations that you will use to test an MS-SMB2 protocol implementation.

###### <a name="5.1.2.2.1"/> 5.1.2.2.1 Create a basic share

This configuration is used to test the basic features of an MS-SMB2 protocol implementation, for example, the **E
