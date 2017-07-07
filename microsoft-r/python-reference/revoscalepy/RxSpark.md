--- 
 
# required metadata 
title: "" 
description: "" 
keywords: "" 
author: "HeidiSteen" 
manager: "" 
ms.date: "" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

## ``RxSpark``


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
class revoscalepy.RxSpark(hdfs_share_dir: str = ‘/user/RevoShare\xadupre’, share_dir: str = ‘/var/RevoShare\xadupre’, user: str = ‘xadupre’, name_node: str = None, port: int = None, auto_cleanup: bool = True, console_output: bool = False, packages_to_load: list = None, idle_timeout: int = 3600, num_executors: int = None, executor_cores: int = None, executor_mem: str = None, driver_mem: str = None, executor_overhead_mem: str = None, extra_spark_config: str = ”, spark_reduce_method: str = ‘auto’, tmp_fs_work_dir: str = ‘/dev/shm’, persistent_run: bool = False, wait: bool = True, **kwargs)
```

