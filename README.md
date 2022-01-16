# hhuann.github.io
# hexo常见命令
- hexo s等价于 hexo server #Hexo 会监视文件变动并自动更新，除修改站点配置文件外,无须重启服务器,直接刷新网页即可生效。
- hexo g 等价于 hexo generate #生成静态网页 (执行 $ hexo g 后会在站点根目录下生成public>文件夹, hexo会将"<font /blog/source/" 下面的.md后缀的文件编译为.html后缀的文件,存放在"/blog/public/ " 路径下)
- hexo d 等价于 hexo deploy #将本地数据部署到远端服务器(如github)
- hexo clean #清除缓存 ,网页正常情况下可以忽略此条命令,执行该指令后,会删掉站点根目录下的public文件夹
# 添加github用户名和邮箱
    git config --global user.name "Name"
    git config --global user.email "Email"
# 生成密匙
    ssh-keygen -t rsa -C "Email"
# 连接github
    ssh -T git@github.com
# 上传文章
    npm i hexo-deployer-git

# 主题原地址
    https://github.com/blinkfox/hexo-theme-matery