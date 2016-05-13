#find命令30个示例

首先，在你的home目录下面创建下面的空文件，来测试下面的find命令示例。
```bash
# vim create_sample_files.sh
touch MybashProgram.sh
touch mycprogram.c
touch MyCProgram.c
touch Program.c
 
mkdir backup
cd backup
 
touch MybashProgram.sh
touch mycprogram.c
touch MyCProgram.c
touch Program.c
 
# chmod +x create_sample_files.sh
 
# ./create_sample_files.sh
 
# ls -R
.:
backup                  MybashProgram.sh  MyCProgram.c
create_sample_files.sh  mycprogram.c      Program.c
 
./backup:
MybashProgram.sh  mycprogram.c  MyCProgram.c  Program.c
```
1. 用文件名查找文件
```bash
# find -name "MyCProgram.c"
./backup/MyCProgram.c
./MyCProgram.c
```
2. 用文件名查找文件，忽略大小写
```bash
# find -iname "MyCProgram.c"
./mycprogram.c
./backup/mycprogram.c
./backup/MyCProgram.c
./MyCProgram.c
```
3. 使用mindepth和maxdepth限定搜索使用指定目录的深度
```bash
# find -mindepth 3 -maxdepth 5 -name passwd
./usr/bin/passwd
./etc/pam.d/passwd
```
4. 在find命令查找到的文件上执行命令
*-exec，find命令对匹配的文件执行该参数所给出的shell命令。相应命令的形式为'command' {  } \;，注意{   }和\；之间的空格*
```bash
#找到MyProgram.c文件 执行复制为abc文件的命令。{}将会被当前文件名取代
find -iname "MyCProgram.c" -exec cp {} abc \;
```
5. 相反匹配
```bash
find -maxdepth 1 -not -iname "MyCProgram.c"
.
./MybashProgram.sh
./create_sample_files.sh
./backup
./Program.c
```
6. 使用inode编号查找文件
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```
