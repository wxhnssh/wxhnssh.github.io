## nvm 常用命令
nvm install latest： 安装最新的 nodejs 版本
nvm install 11.12.0： 安装对应的 nodejs 版本
nvm uninstall 11.12.0：卸载对应的 nodejs 版本
nvm list available： 列出所有可用的 nodejs 版本
nvm list： 查看 nvm 列出已经安装的 nodejs 版本
nvm use 11.12.0： 使用对应的 nodejs 版本

## nrm 常用命令
nrm ls ：查看所有配置好的源以及对应名称
nrm add company http://npm.xxx.cn：添加源，company 是名称，可以自行命名，后面是源的 url 地址
nrm del company ：删除源，根据名称 company 可以删除对应的源
nrm test [registry] ：测试源的速度，不加对应的 registry 名称，测试所有源的速度，添加对应的名称，比如 company，就是测试 company 对应的源的速度
nrm use company ：切换源，即可使用 company 对应名称的源

## 额外知识点
安装 cnpm：npm install -g cnpm --registry=https://registry.npm.taobao.org

设置 npm 全局包的安装路径（如果不想自己控制路径就不需要做下面这些操作）：

执行命令：npm config set prefix "D:\DevTools\Nvm\npm-global"
设置环境变量：将 Path 中： C:\\Users\\Administrator\\AppData\\Roaming\\npm 修改为 D:\\DevTools\\Nvm\\npm-global
查看已经安装的全局包：npm ls -g --depth=0

手动设置 npm 源

npm config get registry ： 查看 npm 当前源
npm config set registry https://registry.npm.taobao.org/：设置 npm 源为淘宝
npm install --registry=https://registry.npm.taobao.org ：使用特定源安装所有依赖的包
npm install express --registry=https://registry.npm.taobao.org：使用特定源安装 express 包