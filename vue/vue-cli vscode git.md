* v这些脚手架必须安装在全局环境中

# 一、老版本

```js
//在全局安装
npm install -g vue-cli
//查看是否安装成功
vue -V

//初始化一个项目
vue init <>
    
    
//开发调试
npm run dev
//打包
npm run build
```

# 二、新版本

```js
//先删除原来的版本
npm uninstall vue-cli -g

//在全局安装
npm install -g @vue/cli
//查看是否安装成功
vue -V


//初始化一个项目
vue create <>

//开发调试
npm run serve

//打包
npm run build

```

# 三、npm

```js
npm install 包名 --save-dev(npm install 包名 -D)
安装的包只用于开发环境，不用于生产环境,保存到devDependencies（开发阶段的依赖）


npm install 包名 --save (npm install 包名 -S)
安装的包需要发布到生产环境的,保存到dependencies(生产阶段依赖)

npm install
安装dependencies和devDependencies中的包


npm uninstall 包名 --save-dev
npm uninstall 包名 --save
npm uninstall 包名 -g    npm uninstall 包名 --global

npm config list 获取npm的配置信息
npm config set proxy=http://.....
npm config get registry 获取下载包的下载地址


```

# 四、vscode中使用git

```bash
//第一次
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/XuWeidongCQ/dms.git
git push -u origin master

//上传到已经存在的仓库
git remote add origin https://github.com/XuWeidongCQ/dms.git
git push -u origin master
```

* 第一次创建好了，之后可以使用vscode自带的git工具