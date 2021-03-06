---
title: What is a Data Science Virtual Machine? | Microsoft Docs
description: Learn the key scenarios, features, and how to get started with Data Science Virtual Machines, an environment and toolkit ready for analytics.
keywords: data science tools, data science virtual machine, tools for data science, linux data science
services: machine-learning
documentationcenter: ''
author: bradsev
manager: jhubbard
editor: cgronlun

ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2016
ms.author: bradsev

---
# <a name="introduction-to-the-data-science-virtual-machine-for-linux-and-windows,-a-cloud-environment-and-toolkit"></a>Introduction to the Data Science Virtual Machine for Linux and Windows, a cloud environment and toolkit
Learn about the key scenarios and features of Azure Data Science Virtual Machines and how to get started. The Data Science VM is a data science environment and toolkit ready for analytics and provisioned in the Microsoft Azure cloud in Windows Server 2012 or OpenLogic 7.2 CentOS-based Linux versions.

## <a name="what-can-i-do-in-the-data-science-virtual-machine?"></a>What can I do in the Data Science Virtual Machine?
The goal of the Data Science Virtual Machine is to provide data professionals at all skill levels and roles with a friction-free data science environment that allows you to start your data science project immediately in a newly created VM instance. This VM saves you considerable time that you would spend if you rolled out a comparable environment on your own.

The Data Science VM is preconfigured for the broad usage scenarios:

* Scale your environment up or down as your project needs change.
* Use any language to program your data science tasks.
* Install other tools and customize the system for your exact needs.

## <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Preconfigured analytics desktop in the cloud
The Data Science VM provides a baseline configuration for data science teams looking to replace their local desktops with a managed cloud desktop. This baseline ensures that all the data scientists on a team have a consistent setup with which to verify experiments and promote collaboration. It also lowers costs by reducing the sysadmin burden and saving on the time needed to evaluate, install, and maintain the various software packages needed to do advanced analytics.  

## <a name="data-science-training-and-education"></a>Data science training and education
Enterprise trainers and educators that teach data science classes usually provide a virtual machine image to ensure that their students have a consistent setup and that the samples work predictably. The Data Science VM creates an on-demand environment with a consistent setup that eases the support and incompatibility challenges. Cases where these environments need to be built frequently, especially for shorter training classes, benefit substantially.

## <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>On-demand elastic capacity for large-scale projects
Data science hackathons/competitions or large-scale data modeling and exploration require scaled out hardware capacity, typically for short duration. The Data Science VM can help replicate the data science environment quickly on demand, on scaled out servers that allow experiments requiring high-powered computing resources to be run.

## <a name="short-term-experimentation-and-evaluation"></a>Short-term experimentation and evaluation
The Data Science VM can be used to evaluate or learn tools such as Microsoft R Server, SQL Server, Visual Studio tools, Jupyter, deep learning / ML toolkits, and new tools popular in the community with minimal setup effort. Since the Data Science VM can be set up quickly, it can be applied in other short-term usage scenarios such as replicating published experiments, executing demos, following walkthroughs in online sessions or conference tutorials.

## <a name="what's-included-in-the-windows-and-linux-versions-of-the-data-science-vm?"></a>What's included in the Windows and Linux versions of the Data Science VM?
The Data Science Virtual Machine has many popular data science and other tools already installed and configured to jump-start building intelligent applications using advanced analytics. These tools can be deployed on the cloud, on-premises, or in a hybrid environment.

It also includes tools that make it easy to work with various Azure data and analytics products. You can explore and build predictive models on large-scale data sets using the Microsoft R Server (Developer Edition) on the VM. A host of other tools from the open source community and from Microsoft are also included, as well as sample code and notebooks.

| Windows Edition | Linux Edition |
| --- | --- |
| Microsoft R Server Developer Edition |Microsoft R Server Developer Edition |
| Anaconda Python 2.7, 3.5 |Anaconda Python 2.7, 3.5 |
| Jupyter Notebook Server (R, Python) |JupyterHub: Multi-user Jupyter notebooks (R, Python, Julia) |
| SQL Server 2016 Developer Edition: Scalable in-database analytics with R services |Postgres, SQuirreL SQL (database tool), SQL Server drivers, and command line (bcp, sqlcmd) |
| Visual Studio Community Edition 2015 (IDE) </br> - Azure HDInsight (Hadoop), Data Lake, SQL Server Data tools </br> - Node.js, Python, and R tools for Visual Studio |IDEs and editors </br> - Eclipse with Azure toolkit plugin </br> - Emacs (with ESS, auctex) gedit |
| Power BI desktop |-- |
| Machine Learning Tools </br> - Integration with Azure Machine Learning </br> - CNTK (deep learning/AI) </br> - Xgboost (popular ML tool in data science competitions) </br> - Vowpal Wabbit (fast online learner) </br> - Rattle (visual quick-start data and analytics tool) </br> - Mxnet (deep learning/AI) |Machine Learning Tools </br> - Integrations with Azure Machine Learning </br> - CNTK (deep learning/AI) </br> - Xgboost (popular ML tool in data science competitions) </br> - Vowpal Wabbit (fast online learner) </br> - Rattle (visual quick-start data and analytics tool) |
| SDKs to access Azure and Cortana Intelligence Suite of services |SDKs to access Azure and Cortana Intelligence Suite of services |
| Tools for data movement and management of Azure and Big Data resources: Azure Storage Explorer, CLI, PowerShell, AdlCopy (Azure Data Lake), AzCopy, dtui (for DocumentDB), Microsoft Data Management Gateway |Tools for data movement and management of Azure and Big Data resources: Azure Storage Explorer, CLI |
| Git, Visual Studio Team Services plugin |Git |
| Windows port of most popular Linux/Unix command-line utilities accessible through GitBash/command prompt |-- |

## <a name="how-to-get-started-with-the-windows-server-data-science-vm"></a>How to get started with the Windows Server Data Science VM
* Create an instance of the VM on Windows by navigating to [this page](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) and selecting the green **Create Virtual Machine** button.
* Sign in to the VM from your remote desktop using the credentials you specified when you created the VM.
* To discover and launch the tools available, click the **Start** menu.

## <a name="get-started-with-the-linux-data-science-vm"></a>Get started with the Linux Data Science VM
* Create an instance of the VM on Linux (OpenLogic CentOS-based) by navigating to [this page](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/) and selecting the **Create Virtual Machine** button.
* Sign in to the VM from an SSH client, such as Putty or SSH Command, using the credentials you specified when you created the VM.
* In the shell prompt, enter dsvm-more-info.
* For a graphical desktop, download the X2Go client for your client platform [here](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) and follow the instructions in the Linux Data Science VM document [Provision the Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).

## <a name="next-steps"></a>Next steps
### <a name="for-the-windows-data-science-vm"></a>For the Windows Data Science VM
* For more information on how to run specific tools available on the Windows version, see [Provision the Microsoft Data Science Virtual Machine](machine-learning-data-science-provision-vm.md) and
* For more information on how to perform various tasks needed for your data science project on the Windows VM, see [Ten things you can do on the Data science Virtual Machine](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-the-linux-data-science-vm"></a>For the Linux Data Science VM
* For more information on how to run specific tools available on the Linux version, see [Provision the Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md).
* For a walkthrough that shows you how to perform several common data science tasks with the Linux VM, see [Data science on the Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-walkthrough.md).

<!--HONumber=Oct16_HO2-->


