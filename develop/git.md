

# git常见问题

[TOC]



### git 错误

- NotSupportedException encountered. ServicePointManager ▒▒֧▒־▒▒▒ localhost ▒▒▒▒▒Ĵ▒▒▒	
    - git config --global http.proxy "127.0.0.1:1080"
- 

### hook

1. 添加项目部署公钥

    1. ssh-keygen -t rsa -C "981037735@qq.com"
    2. cat ~/.ssh/id_rsa.pub

2. git ssh 获取代码

3. php接收git回调

    - password 验证密码
    - ref 验证push分支
    - shell_exec可执行
    - cd /home/NyApi && pwd && git pull origin prod 2>&1  切换到执行目录，并输出当前目录，输出一切错误信息
    - `git执行权限大于shell_exec 运行权限时，push操作无法执行`

    ```
    if('Nypush' == $_POST['password']
                && 'refs/heads/prod' == $_POST['ref']
            ){
            	//记录到日志
                Logger::getInstance()->log(shell_exec('cd /home/NyApi && pwd && git pull origin prod 2>&1') ?: '','git_log');
            }
            else{
                Logger::getInstance()->log('ERR, data: '.json_encode((array)$this->request()->getRequestParam()),'git_log');
            }	
    ```
