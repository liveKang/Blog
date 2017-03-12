# 使用node版本管理工具-NVM

![](http://i1.piimg.com/567571/b5a448fa0c01e5bc.jpg)

之前一直使用的[NodeJs](https://nodejs.org/en/)稳定版本（6～），没毛病都挺正常的，node_modules目录也比较清晰，后来学习ES6，7时，想在node环境下测试下ES6，7新特性，发现只能在最新的Current版本里面，开启harmony模式，于是就升级了node版本（7～）。

然后就在项目上遇到了node模块上的问题，node-sass这个一般都是要重新安装的，还有其他的一些问题。我推断时由于node版本引起的，而且在安装最新版本current的node之后，npm install的node_modules目录安装了很多其他的模块上来，目录结构没有稳定版本清晰。基于以上两点，试试node版本管理工具[nvm](https://github.com/creationix/nvm)，发现真的很好用，好东西分享下。

nvm 是 Mac 下的 node 管理工具，有点类似管理 Ruby 的 rvm，如果是需要管理 Windows 下的 node，官方推荐是使用 [nvmw](https://github.com/hakobera/nvmw) 或 [nvm-windows](https://github.com/coreybutler/nvm-windows) 。

以下具体说下 Mac 系统中的安装与使用细节（Windows 系统仅供类比参考）。

### (一) 卸载安装到全局的 node／npm
之前的很多模块，诸如gulp，cordova，ionic等等，都是安装在全局目录下，现在需要管理node版本，自然也就需要重新安装对应node版本下的全局模块。

node 命令在 /usr/local/bin/node ，npm 命令在全局 node_modules 目录中，具体路径为 /usr/local/lib/node_modules/npm

安装nvm之前，先删除已安装的node和全局node模块：

<pre>
npm ls -g --depth=0 #查看已经安装在全局的模块，以便删除这些全局模块后再按照不同的 node 版本重新进行全局安装
sudo rm -rf /usr/local/lib/node_modules #删除全局 node_modules 目录
sudo rm -rf /usr/local/bin/node #删除 node
cd  /usr/local/bin && ls -l | grep "../lib/node_modules/" | awk '{print $9}'| xargs rm #删除全局 node 模块注册的软链
</pre>

### (二) 安装nvm

<pre>
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash
</pre>

安装完成后请重新打开终端环境，Mac下推荐使用[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)代替默认的 bash shell。

curl 完成后nvm就被安装在了~/.nvm下了，接下来就需要配一下环境变量了，如果你也使用了zsh的话，就需要在~/.zshrc这个配置文件中配置，否则就找找看~/.bash_profile或者~/.profile

输入`nano .zshrc`打开~/.zshrc，在最后一行加上：
<pre>
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
</pre>

测试：终端输入`nvm`,有Node Version Manager等一大段出现就对了。

### (三) 安装切换各版本 node/npm

<pre>
nvm install stable #安装最新稳定版 node，现在是 6.10.0
nvm install 7.7.2 #安装 7.7.2 版本

# 特别说明：以下模块安装仅供演示说明，并非必须安装模块
nvm use 7 #切换至 7.7.2 版本
npm install -g webpack #安装 webpack 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v7.7.2/lib/webpack
nvm use 6 #切换至 6.10.0 版本
npm install -g react-native-cli #安装 react-native-cli 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v6.10.0/lib/react-native-cli

nvm alias default 6.10.0 #设置默认 node 版本为 6.10.0
</pre>

### (四) 使用 .nvmrc 文件配置项目所使用的 node 版本

如果你的默认 node 版本（通过 nvm alias 命令设置的）与项目所需的版本不同，则可在项目根目录或其任意父级目录中创建 .nvmrc 文件，在文件中指定使用的 node 版本号，例如：

<pre>
cd <项目根目录>  #进入项目根目录
echo 6 > .nvmrc #添加 .nvmrc 文件
nvm use #无需指定版本号，会自动使用 .nvmrc 文件中配置的版本
node -v #查看 node 是否切换为对应版本
</pre>

![](http://i1.piimg.com/567571/95f0eded2d8c651b.png)

<h3>哈哈哈，SO EASY！ ![](http://p1.bpimg.com/567571/04a271d44c285b5a.jpg) 

###（附录）参考资料
* [nvm](https://github.com/creationix/nvm)
* [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)
* [nodeJs](https://nodejs.org/en/)
* [使用 nvm 管理不同版本的 node 与 npm](http://www.cnblogs.com/kaiye/p/4937191.html)
* [oh-my-zsh配置你的zsh提高shell逼格终极选择](http://yijiebuyi.com/blog/b9b5e1ebb719f22475c38c4819ab8151.html)
*  [TJ大神的n](https://github.com/tj/n)

