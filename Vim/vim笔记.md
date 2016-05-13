# vim笔记
全局配置
```bash
vi /etc/vim/vimrc
```
显示行号
```bash
:set nu
```
搜索高亮
```bash
:set hlsearch
```
搜索统计
```bash
:%s/xxxx//gn
```
搜索替换
```bash
:%s/a/b/ 替换第一行第一个a为b
:%s/a/b/g 替换所有行的所有a为b
```

撤销和反撤销
```bash
	u   撤销上一步的操作
	Ctrl+r 恢复上一步被撤销的操作
```

