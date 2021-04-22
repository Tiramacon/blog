---
title: Github SSH相关配置
date: 2020-07-03 13:58:58
categories:
- Tool
tags: 
- git
---

- [配置 github ssh key](#配置-github-ssh-key)
- [解决 ssh: connect to host github com port 22: timed out](#解决-ssh-connect-to-host-github-com-port-22-timed-out)

## 配置 github ssh key

使用 Git Bash 生成 ssh key

- 保证在 ~ 路径下

  `$ cd ~`

- 命令生成 ssh

  ```bash
  $ ssh-keygen -t rsa -C "xxxxxx@yy.com"  #填写个人邮箱
  Generating public/private rsa key pair.
  Enter file in which to save the key (/c/users/usersName/.ssh/id_rsa):  #回车
  Enter passphrase (empty for no passphrase):  #输入密码 (可以为空)
  Enter same passphrase again:  #再次确认密码 (可以为空)
  Your identification has been saved in /c/Users/xxxx_000/.ssh/id_rsa.
  Your public key has been saved in /c/Users/xxxx_000/.ssh/id_rsa.pub.
  ```

在 github 添加 ssh key

- 在 C:/users/usersName/.ssh/目录下，复制 id_rsa.pub 文件的内容
- 在 github 设置页面的 ssh key 栏点击 new ssh key 粘贴内容

## 解决 ssh: connect to host github com port 22: timed out

使用 Git Bash 检查 ssh 连接

- 输入命令检查

  ```bash
  $ ssh -T git@github.com
  ssh: connect to host github.com port 22: Connection timed out  #报错
  ```

- 报错则检测 ssh key 文件是否存在

  ```bash
  $ cd ~/.ssh
  $ ls  #查看当前目录文件
  id_rsa id_rsa.pub  #若不存在两个文件则重新配置ssh key
  ```

- 若存在 ssh key 文件仍报错则新建 config 文件

  1. 新建 config 文件

  ```bash
  $ cd ~/.ssh
  $ vim config
  ```

  2. 按 i 键进入输入模式，编辑以下内容：

  ```bash
  Host github.com
  User xxxxxx@yyy.com  #个人邮箱
  Hostname ssh.github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa
  Port 443
  ```

  3. 按两次 ESC 键退出输入模式，按 :wq 保存并退出
  4. 再次检查 ssh 连接

  ```bash
  $ ssh -T git@github.com
  Connection reset by ip port 443  #若出现连接重设提示则再次检查
  $ ssh -T git@github.com
  The authenticity of host '[ssh.github.com]:443 ([192.30.253.122]:443)' can not be established.
  RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
  Are you sure you want to continue connecting (yes/no)? yes  #输入yes并回车
  Warning: Permanently added '[ssh.github.com]:443,[192.30.253.122]:443' (RSA) to the list of known hosts.
  Hi Veal98! You are successfully authenticated, but GitHub does not provide   shell access.  #连接成功
  ```
