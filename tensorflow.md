### config && use

------

####激活virtualenv

```shell
# 建立全新的virtualenv 
$ virtualenv --system-site-packages ~/tensorflow
$ cd ~/tensorflow

$ source bin/activate  # 如果使用 bash
$ source bin/activate.csh  # 如果使用 csh
(tensorflow)$  # 终端提示符应该发生变化

# 使用完tensorflow后退出
(tensorflow)$ deactivate
```

