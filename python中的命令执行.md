
# python中的命令执行

## 命令执行函数
1. `os.system('whoami')`
2. `os.popen('whoami').readlines()`
3. `import commands
result = commands.getoutput('whoami') //python2`
4. `import subprocess
subprocess.call('ipconfig')`
## 动态导入模块
1. `__import__('os').popen('whoami').readlines()`
2. `import importlib
aa = importlib.import_module('os')
print aa.popen('whoami').readlines()`
3. `reload(os)
print os.popen('whoami').readlines() //python2`
4. `import imp
fp, pathName, description = imp.find_module('os')
imp.load_module('os', fp, pathName, description)
print os.popen('ls').readlines()`
#### 参考链接
[t00ls](https://www.t00ls.net/thread-46778-1-1.html)