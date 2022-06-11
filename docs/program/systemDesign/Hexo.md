# Hexo博客搭建

1.安装Hexo运行环境

首先我们要安装Node.js，安装非常简单，Node.js安装完成后的检验操作是在DOS窗口下输入命令 `node -v`和`npm -v`，这两个命令就是查看版本信息的。 

2.安装hexo框架 

可以使用国内的镜像源安装：`npm install -g cnpm --registry=https://registry.npm.taobao.org `

接下来正式安装：

`cnpm install -g hexo-cli`

可以使用`hexo -v`验证一下，然后新建一个文件夹，用于存放博客的文件，在安装了git之后，右键选择git bash here ，弹出Git Bash窗口 ，执行`hexo init`命令初始化，`hexo g`编译生成静态页面，然后用`hexo s`启动博客在浏览器中访问`http://localhost:4000`，至此安装成功。

如果想写一篇文章，使用`hexo n "HelloWorld"`,在`blog\source\_posts`目录下去编辑即可。

3.部署到github

在github上新建一个仓库，仓库名必须是`用户名.github.io`

为Hexo安装GIt插件` cnpm install hexo-deployer-git --save `

修改 _config.yml 配置文件如下：

```yaml
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/xxx/xxx.github.io.git
  branch: master
```

部署到远端，使用`hexo d`命令，就OK了。

4.博客主题优化

（1）更换博客主题 [yilia主题官网](https://github.com/litten/hexo-theme-yilia)

可以去查看自己喜欢的主题 https://hexo.io/themes/ 

我们下载yilia这个主题`git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia`

然后修改hexo根目录下的 `_config.yml` ： `theme: yilia` 即可。

（2） [matery主题官网]( https://github.com/blinkfox/hexo-theme-matery)

修改主题的logo图和favicon图标

```yaml
favicon: /favicon.png
logo: /favicon.png
```

去掉右上角的github图标，打开你的主题配置文件，找到下面的配置:

```yaml
# 配置是否在 header 中显示 fork me on github 的图标，默认为true，你可以修改为你的仓库地址.
githubLink:
  enable: true
  url: https://github.com/blinkfox/hexo-theme-matery
  title: Fork Me
```

去掉主页的Github按钮，打开主题配置文件，找到下面的配置： 将enable属性值由true改为fals即可。 

```yaml
# 首页 banner 中的第二个按钮的配置，包括按钮的显示名称、font awesome图标和按钮的超链接.
indexbtn:
  enable: true
  name: Github
  icon: fab fa-github-alt
  url: https://github.com/blinkfox/hexo-theme-matery
```

修改社交链接

```yaml
# 首页 banner 中的第二行个人信息配置，留空即不启用
socialLink:
		...
```

修改导航栏颜色及透明效果:
打开`themes/hexo-theme-matery/source/css/matery.css`文件，在252行，有一个`.bg-color`属性，修改其属性值即可，代码如下：

```yaml
.bg-color {
    background-image: linear-gradient(to right, #4cbf30 0%, #0f9d58 100%); //修改成自己喜欢的颜色值
    opacity: 0.8;  //透明效果 值范围 0~1，看情况自己修改
}
```

修改banner图和文章特色图:
你可以直接在 `/source/medias/banner `文件夹中更换你喜欢的 banner 图片，主题代码中是每天动态切换一张，只需 7 张即可。如果你会 JavaScript 代码，可以修改成你自己喜欢切换逻辑，如：随机切换等，banner 切换的代码位置在 `/layout/_partial/bg-cover-content.ejs `文件的 `<script></script> `代码中：

```javascript
$('.bg-cover').css('background-image', 'url(/medias/banner/' + new Date().getDay() + '.jpg)');
```

在 `/source/medias/featureimages `文件夹中默认有 24 张特色图片，你可以再增加或者减少，并需要在 _config.yml 做同步修改。如果想改为每小时或者每分钟切换banner图的话，需要将`getDay()`改为`getHours()`或者`getMinutes()`即可。

添加首页动态打字效果副标题:

```yaml
# 打字效果副标题.
# 如果有符号 ‘ ，请在 ’ 前面加上 \
subtitle:
		...
```

大致的美化工作就完成了！小伙伴不满意的话可以自己在定义。

