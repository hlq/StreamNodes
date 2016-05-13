# SVN笔记
## 错误解决方案
### E200014:
在错误文件所在目录执行：
```bash
svn update --set-depth empty
svn update --set-depth infinity  
```

