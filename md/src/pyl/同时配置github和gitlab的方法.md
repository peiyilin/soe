
# 背景
    公司的代码是托管在gitlab上，同时我们还想访问github管理自己的项目代码，此时就需要配置了
# 思路
	1、在~/.ssh/下生成两个本地秘钥文件（github:id_rsa_self;gitlab:id_rsa_company）
	2、新增一个配置文件config
	3、将本地秘钥拷贝到不同的平台SSH-key的配置中
# 步骤
	1、生成秘钥
	#在~/.ssh/目录会生成id_rsa_company和id_rsa_self私钥和公钥。
	#后面将id_rsa_company.pub中的内容粘帖到公司GitLab服务器的SSH-key的配置中。
	$ ssh-keygen -t rsa -C "注册gitlab邮箱" -f ~/.ssh/id_rsa_company
	$ ssh-keygen -t rsa -C "注册github邮箱" -f ~/.ssh/id_rsa_self
	2、创建config
	#在~/.ssh下创建config文件,在Windows下可以鼠标操作创建，但是要注意，这个文件是没有后缀的。
	cd ~/.ssh
	touch config
	添加如下内容：
	#在设置HostName需要注意：
	#我们获取公司gitlab或者github地址的时候，可能是copy浏览器上的地址而来，就像这样的https://github.com，
	#这个时候需要把https://去掉，保留github.com部分，不然是连不上的。
	Host github.com    
		Port 22
		User git
		HostName github.com   #github地址
		PreferredAuthentications publickey
		IdentityFile ~/.ssh/id_rsa_self
	Host gitlab.com #公司gitlab地址
		Port 22
		User git
		HostName gitlab.com   #公司gitlab地址
		PreferredAuthentications publickey
		IdentityFile ~/.ssh/id_rsa_company
	3、测试
	# 测试github
    	$ ssh -T git@github.com
    
    	# 测试gitlab
	$ ssh -T git@gitlab.com
	注意：The authenticity of host 'git.blingabc.com (120.27.12.76)' can't be established.
	ECDSA key fingerprint is SHA256:qopHbX8WJMeXPn8U9dkfwH/6yc9b9eCMxOaU6eDQMVE.
	Are you sure you want to continue connecting (yes/no)? yes
	出现这句话后一定要输入yes，直接敲回车会失败
	
