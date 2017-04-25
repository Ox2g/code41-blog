title: NVM默认Node设置
date: 2017-04-24 09:47:47
tags:
- NodeJS
- NVM
categories:
- NodeJS
---
#### 问题：安装NVM后，每次重启Shell后都需要使用"nvm use"重新设置node版本
使用"nvm alias default"设置默认node版本后避免shell重启后默认nvm的node版本失效问题
```shell
nvm alias default v6.10.2
```

