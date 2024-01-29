
> 给这个文件的所有者增加可执行权限 #Linux权限 

```shell
chmod u+x test.sh 
```

> 遍历一个目录下的文件，做备份
> 备份的文件名增加一个年月日的后缀，比如备份为test.txt_20231116

> 第一行的内容指定了shell脚本解释器的路径，而且这个指定路径只能放在文件的第一行。第一行写错或者不写时，系统会有一个默认的解释器进行解释
```shell
#! /bin/bash
suffix=`date +%Y%m%d`
for f in `find /home/ -type f -name "*.txt"`
do
        echo "备份文件$f"
        cp ${f} ${f}_${suffix}
done
```
