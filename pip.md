# 1、pip下载安装 #
## 1.1 pip下载 ##



    # wget "https://pypi.python.org/packages/source/p/pip/pip-1.5.4.tar.gz#md5=834b2904f92d46aaa333267fb1c922bb" --no-check-certificate
## 1.2 pip安装 ##


	# tar -xzvf pip-1.5.4.tar.gz
	# cd pip-1.5.4
	# python setup.py install
	
# 2. pip使用详解 #
## 2.1 pip安装包 ##


	# pip install SomePackage
 	 [...]
  	Successfully installed SomePackage
  	
## 2.2 pip查看已安装的包 ##


	# pip show --files SomePackage
  	Name: SomePackage
  	Version: 1.0
  	Location: /my/env/lib/pythonx.x/site-packages
  	Files:
   	../somepackage/__init__.py
   	[...]
   	
## 2.3 pip检查哪些包需要更新 ##


	# pip list --outdated
  	SomePackage (Current: 1.0 Latest: 2.0)
  	
## 2.4 pip升级包 ##


	# pip install --upgrade SomePackage
 	 [...]
  	Found existing installation: SomePackage 1.0
  	Uninstalling SomePackage:
    Successfully uninstalled SomePackage
 	 Running setup.py install for SomePackage
 	 Successfully installed SomePackage
 	 
## 2.5 pip卸载包 ##


    $ pip uninstall SomePackage
      Uninstalling SomePackage:
    /my/env/lib/pythonx.x/site-packages/somepackage
      Proceed (y/n)? y
      Successfully uninstalled SomePackage
      
# 3. pip使用实例 #
## 3.1 安装redis ##


	# pip install redis
	
## 3.2 卸载redis ##


	# pip uninstall redis
	Uninstalling redis:
 	 /usr/lib/python2.6/site-packages/redis-2.9.1-py2.6.egg-info
	.....省略一些内容....
	Proceed (y/n)? y
  	Successfully uninstalled redis
  	
## 3.3 查看待更新包 ##


    pip list --outdate
    pygpgme (Current: 0.1 Latest: 0.3)
    pycurl (Current: 7.19.0 Latest: 7.19.3.1)
    iniparse (Current: 0.3.1 Latest: 0.4)
    
# 4. 常见错误 #
     ImportError No module named setuptools 
    请参考《ImportError No module named setuptools解决》

# 5. pip参数解释 #


	# pip --help
 
    Usage:   
      pip <command> [options]
     
    Commands:
      install 安装包.
      uninstall   卸载包.
      freeze  按着一定格式输出已安装包列表
      list列出已安装包.
      show显示包详细信息.
      search  搜索包，类似yum里的search.
      wheel   Build wheels from your requirements.
      zip 不推荐. Zip individual packages.
      unzip   不推荐. Unzip individual packages.
      bundle  不推荐. Create pybundles.
      help当前帮助.
     
    General Options:
      -h, --help  显示帮助.
      -v, --verbose   更多的输出，最多可以使用3次
      -V, --version   现实版本信息然后退出.
      -q, --quiet 最少的输出.
      --log-file <path>   覆盖的方式记录verbose错误日志，默认文件：/root/.pip/pip.log
      --log <path>不覆盖记录verbose输出的日志.
      --proxy <proxy> Specify a proxy in the form [user:passwd@]proxy.server:port.
      --timeout <sec> 连接超时时间 (默认15秒).
      --exists-action <action>Default action when a path already exists: (s)witch, (i)gnore, (w)ipe, (b)ackup.
      --cert <path>   证书.