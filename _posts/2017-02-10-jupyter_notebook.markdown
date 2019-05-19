---
layout: post
category: "python"
title:  "Set up config for jupyter notebook"
tags: [python, jupyter, notebok]
---

- TOC
{:toc}

## jupyter notebook的配置

### 新建配置文件

参考[这篇文章](https://segmentfault.com/a/1190000009305646)，对自己的notebook进行配置，方便快捷。比如可以在打开notebook时直接先加载一些模块等。

~~~bash
# creat新建配置文件
gongjing@MBP ~/.ipython/profile_default % ipython profile create
[ProfileCreate] Generating default config file: u'/Users/gongjing/.ipython/profile_default/ipython_config.py'
[ProfileCreate] Generating default config file: u'/Users/gongjing/.ipython/profile_default/ipython_kernel_config.py'
gongjing@MBP ~/.ipython/profile_default % ll
total 16304
drwxr-xr-x  2 gongjing  staff    68B Jun 18  2016 db
-rw-r--r--  1 gongjing  staff   7.9M Feb 10 09:42 history.sqlite
-rw-r--r--  1 gongjing  staff    22K Feb 10 09:43 ipython_config.py
-rw-r--r--  1 gongjing  staff    18K Feb 10 09:43 ipython_kernel_config.py
drwxr-xr-x  2 gongjing  staff    68B Jun 18  2016 log
drwx------  2 gongjing  staff    68B Jun 18  2016 pid
drwx------  2 gongjing  staff    68B Jun 18  2016 security
drwxr-xr-x  3 gongjing  staff   102B Jun 18  2016 startup
~~~

ipython_config.py 这个文件是配置ipython的；

ipython_kernel_config.py 这个文件是配置notebook的；

### 编辑配置文件

加载常用模块，matplot设置成inline

~~~python
# Configuration file for ipython-kernel.

c = get_config()

c.InteractiveShellApp.exec_lines = [
        "import pandas as pd",
        "import numpy as np",
        "import scipy.stats as spstats",
        "import scipy as sp",
        "import matplotlib.pyplot as plt",
        "import seaborn as sns",
        "sns.set(style='white')",
        "sns.set_context('poster')",
        ]

c.IPKernelApp.matplotlib = 'inline'
~~~

### Connect notebook server

```bash
# login the server
$ ssh -X -p 12000 zhangqf7@166.111.152.116

$ export PYTHONPATH=~/anaconda2/lib/python2.7/site-packages

# specify a port
$ jupyter-notebook --port 9988

# local
$ ssh -N -f -L localhost:9987:localhost:9988 zhangqf7@166.111.152.116 -p 12000

# type url: localhost:9987
# may need token
```