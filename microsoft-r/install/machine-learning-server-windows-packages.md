---
# required metadata
title: "Add or remove R and Python on Machine Learning Server for Windows"
description: "How to install or uninstall R and Python packages on Machine Learning Server for Windows."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "07/31/2018"
ms.topic: "conceptual"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---

# How to add or remove R and Python packages on Machine Learning Server for Windows

**Applies to:  Machine Learning Server 9.2.1 | 9.3**

You can add open-source and third-party R and Python packages to the same local repository containing the product-specific packages for Machine Learning Server, allowing you to call functions from any library in the same script. Any packages you add to the repository must be compatible with the base R and Anaconda distributions upon which Microsoft's R and Python libraries are built.

## Version requirements

Product-specific packages like RevoScaleR and revoscalepy are built on base libraries of R and Python respectively.  Any new packages that you add must be compatible with the base libraries installed with the product. Upgrading or downgrading the version of R or Python installed by Machine Learning Server setup is not supported.

Base R is distributed through Microsoft R Open, as installed by Machine Learning Server or R Server. Python is distributed though Anaconda, also installed by Machine Learning server.

| Product version | R version | Anaconda/Python version |
|-----------------|-----------|-------------------------|
| 9.3             | 3.4.3 |  4.2/3.5 |
| 9.2             | 3.4.1 |  4.2/3.5 |
| 9.1             | 3.3.3 |  not applicable |

To verify the base library versions on your system, start a command line tool and check the version information displayed when the tool starts. 

1. For R, start **R.exe** from \Program Files\Microsoft\ML Server\R_SERVER\bin\x64.

   Loading the R execution environment displays base R version information, similar to this output.

   `R version 3.4.3 (2017-11-30) -- "Kite-Eating Tree"
   Copyright (C) 2017 The R Foundation for Statistical Computing
   Platform: x86_64-w64-mingw32/x64 (64-bit)`


2. For Python, start **Python.exe** from \Program Files\Microsoft\ML Server\PYTHON_SERVER.

   Loading the R execution environment displays base R version information, similar to this output.

   `Python 3.5.2 |Anaconda 4.2.0 (64-bit)| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)] on win32
    Type "help", "copyright", "credits" or "license" for more information.`

## Package location

On Linux, packages installed and used by Machine Learning Server can be found at these locations:

+ For R: `/opt/microsoft/mlserver/9.3.0/`

+ For Python: `/opt/microsoft/mlserver/9.3.0/runtime/python/pkgs`

## Add or remove R packages

R packages tend to have with multiple dependencies so we generally recommend using a tool like miniCran. For more information and alternative methodologies, see [R package management](../operationalize/configure-manage-r-packages.md).

## Add or remote Python packages

On a command prompt with elevated permissions, install new Python 3.5 compatible packages by running the following command: 

   `C:\Program Files\Microsoft\ML Server\PYTHON_SERVER\Scripts\pip.exe install <packagename>`

Uninstall packages by reversig the action using the same executable (also with elevated permissions): 

   `C:\Program Files\Microsoft\ML Server\PYTHON_SERVER\Scripts\pip.exe uninstall <packagename>`

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)