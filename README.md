# live2d

#### 介绍
使用live2d安装到网站教程，超详细！超简单！（附带多种模型）


#### 安装教程

1.  先把必要的三个文件夹下载下来，然后上传到服务器网站目录（ps:这里的目录文件必须是可以通过链接访问的）
![输入图片说明](images/image.png)

2.  在你的网站首页文件index.html中插入以下代码 (ps:记得看准文件路径)

 `在头部<head>标签内引入css样式`

```
<link rel="stylesheet" href="/live2d/css/live2d.css" />
<link rel="stylesheet" href="/live2d/css/waifu.css" />
```

 `在尾部<body>标签内引入js及初始化live2d`
```
<script type="text/javascript" src="live2d/js/jquery.min.js"></script>
<script type="text/javascript" src="live2d/js/live2d.js"></script>
<script type="text/javascript" src="live2d/js/waifu-tips.js"></script>
<script type="text/javascript">
initWidget({
	waifuPath: "/live2d/waifu.json",
	cdnPath: "/live2d/"
});
</script>
```

3. 两步就完成了，刷新你的页面，去看看效果吧啊(●ˇ∀ˇ●)

`ps: 鼠标放在页面某个元素上时，需要 Live2D 看板娘提示的请修改 live2d/waifu.json 文件`


### wordpress用户安装教程

1. 上面的安装教程也可用于wordpress, 只要进入 `wordpress根目录/wp-content/themes/正在使用的主题名` 此目录，然后找到里面的 `header.php`以及`footer.php`文件引入以上的css和js即可

2. 不过第一种方法只能在wordpress首页显示, 接下来介绍一种可以全局显示的方法：

    (1) 首先进入主题文件的根目录，创建一个`js`文件夹

    (2) 在文件夹中创建一个`autoload.js`文件，并写入以下代码加载所需文件（ps:文件在仓库中也有，可直接复制）

        // 注意：live2d_path 参数应使用绝对路径
        const live2d_path = "http://域名/live2d/";
        //const live2d_path = "/live2d-widget/";
        
        // 封装异步加载资源的方法
        function loadExternalResource(url, type) {
            return new Promise((resolve, reject) => {
                let tag;
        
                if (type === "css") {
                    tag = document.createElement("link");
                    tag.rel = "stylesheet";
                    tag.href = url;
                }
                else if (type === "js") {
                    tag = document.createElement("script");
                    tag.src = url;
                }
                if (tag) {
                    tag.onload = () => resolve(url);
                    tag.onerror = () => reject(url);
                    document.head.appendChild(tag);
                }
            });
        }
        
        // 加载 waifu.css live2d.js waifu-tips.js
        if (screen.width >= 768) {
            Promise.all([
                loadExternalResource(live2d_path + "css/waifu.css", "css"),
                loadExternalResource(live2d_path + "css/live2d.css", "css"),
                loadExternalResource(live2d_path + "js/live2d.js", "js"),
                loadExternalResource(live2d_path + "js/waifu-tips.js", "js")
            ]).then(() => {
                initWidget({
                    waifuPath: live2d_path + "waifu.json",
                    cdnPath: live2d_path
                });
            });
        }
        // initWidget 第一个参数为 waifu-tips.json 的路径，第二个参数为 API 地址
        // 初始化看板娘会自动加载指定目录下的 waifu-tips.json


    (3) 在主题根目录下的 `functions.php` 文件中添加以下代码

        function load_custom_script() {
            wp_enqueue_script(
                'autoloadLive2d', // 注册的脚本名称
                get_template_directory_uri() . '/js/autoload.js', // 公共 JS 文件的路径
                array(), // 依赖项数组
                '1.0', // 版本号
                true // 是否在页面底部加载
            );
        }
        add_action('wp_enqueue_scripts', 'load_custom_script');


        上述代码中，我们使用了 wp_enqueue_script() 函数来注册并加载公共的 JavaScript 文件。
        其中，第一个参数是注册的脚本名称，可以任意指定；
        第二个参数则是公共 JS 文件的路径，我们使用了 get_template_directory_uri() 函数来获取主题根目录的路径，并在其后面加上 js/autoload.js 来指定公共 JS 
        文件的路径；
        第三个参数是依赖项数组，可以为空数组；
        第四个参数是版本号，可以任意指定；
        第五个参数则表示是否在页面底部加载 JS 文件，这里我们将其设置为 true，表示在页面底部加载。

    (4) 其实到这里就完成了，效果已经出来了；若没效果的话可以试试在`header.php`或`footer.php`文件中调用以上写的函数

        <?php wp_enqueue_script('autoloadLive2d'); ?>

#### 功能介绍

看板娘上的按钮功能包括聊天、飞机大战、变装、拍照、更多、关闭。。。等等

效果演示：http://www.jokerpan.cn

`持续更新模型中...`
