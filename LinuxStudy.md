# 1. 4个环境变量文件
### 1. /etc/profile: 操作系统定制用户环境时使用的第一个文件,为每个用户设置环境信息,当用户第一次登录时执行该文件

### 2. /etc/environment: 登录时系统使用的第二个文件,系统在读取自己的profile前.设置环境文件的环境变量

### 3. ~/.bash_profile: 在登录时用到的第三个文件,用于设置当前用户的shell信息,用户登录时仅执行一次,默认情况下设置一些环境变量,执行用户的.bashrc文件

### 4. ~/.bashrc: 该文件包含专用于当前用户的bash shell的bash信息,登录时以及每次打开新的shell时,该文件会被提取