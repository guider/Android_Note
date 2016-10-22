##解决OSX使用oh-my-zsh后.bash_profile自定义失效


* 为了使OSX自带的终端在使用上更加顺手，便安装了oh-my-zsh插件，
但发现之前在.bash_profile自定义的一些内容都失效了。

###问题分析

>oh-my-zsh有自己的配置文件，覆盖了.bash_profile的内容。



###解决
>vi ~/.zshrc，在最后一行加入source ~/.bash_profile，
这样就可以”继承”.bash_profile的配置了。
执行source ~/.zshrc，让配置生效，重新使用，一切OK！
