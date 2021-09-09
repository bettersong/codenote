#### 切换用户

````shell
su 用户名
切换到root，直接su
````

#### 管道过滤

````shell
ls /etc | grep passwd
````

#### 查看内部内容

````shell
more /etc/passwd
````

#### 创建组

````shell
addgroup team(组名)   (只有root有权限)
查看  more /etc/team
````

#### 更改组

````shell
usermod -g 组ID 用户
# 
````

#### 删除组

````shell
groupdel 组名

#查看
more /etc/group
````

#### 查看所属目录

````shell
pwd
````

#### 查看

````shell
ls -a /etc/skel
````

#### 复制

````shell
cp /etc/passwd ~/a.txt
````

