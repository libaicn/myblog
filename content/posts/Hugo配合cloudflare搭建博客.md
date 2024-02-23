+++
title = 'Hugo配合cloudflare搭建博客'
date = 2024-02-23T20:52:39+08:00
draft = true
+++

折腾了一天hugo搭建博客, 写一篇文章用以记录.

### hugo安装

在Mac下直接使用brew安装(其他系统可以在[官网](https://gohugo.io/installation/)按照系统安装:

```brew install hugo```

### Hugo设置

1. 创建站点

    创建站点就是创建博客的工作目录

    ```hugo new site /path/to/site```

    然后进入目录:

    ```cd /path/to/site```

2. 创建第一篇博客

    ```hugo new posts/hello_world.md```

    第一篇博客就创建好了, 格式如下:

    ```
    +++
    title = 'Hello_world'
    date = 2024-02-23T13:45:11+08:00
    draft = true
    +++

    ### 正文内容
    1. aaa
    2. bbb
    3. ccc
    ```

+++之间的内容是toml格式, 你也可以改成yaml格式`---`或者JSON格式

3. 安装主题

    这里我推荐的是[PaperMod](https://github.com/adityatelange/hugo-PaperMod/)
    
    安装命令:
    ```
    git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod

    git submodule update --init --recursive # needed when you reclone your repo (submodules may not get cloned automatically)
    ```

4. 运行hugo

    该命令需要再hugo根目录执行, 就是第1点创建的目录

    ```
        hugo server -D
    ```
    
    执行后打印日志里面会有访问地址,一般是`http://localhost:1313/`

### 将博客提交到GitHub

GitHub账号自备

1. 创建一个新仓库, 我这里叫`myblog`

2. 在博客目录是用Git初始化

    ```
    在/path/to/site目录下执行
    git init .
    git remote add origin https://github.com/xxx/myblog.git # 这里是你博客仓库的地址
    git add -A 
    git commit -m "first commit"
    git push -u origin master
    ```



### 部署到Cloudflare

cloudflare账户需要自己创建.




1. 点击`Workers和Pages`
![1](https://missuo.ru/file/519a237c20b07b5ddc774.png)

2. 点击创建应用程序(如果是第一次应该是有个创建的指引,我记不清了)
![2](https://missuo.ru/file/c50f126378182e189d959.png)

3. 点击Pages, 然后点击链接到Git, 选择刚刚创建的厂库, 我这里是`myblog`
![3](https://missuo.ru/file/57a3ce86830ec81b65d06.png)

4. 点击最下面的开始设置, 然后构建设置选hugo
![4](https://missuo.ru/file/04da037c6449774fae6f9.png)

5. 点击保存并部署, 至此部署完成

6. 部署完成后,会有个xxx.pages.dev的地址,就是cloudflare给你分配的免费地址, 可以点击访问你的博客.
![6](https://missuo.ru/file/eb90780668ac1e2c60916.png)


### 自定义域名

如果你有自己的域名, 可以把自己的域名绑定到站点上面去.

1. 点击自定义域, 输入自己的域名
![6.1](https://missuo.ru/file/a2b94d2697cb47a7d99de.png)

2. 我的域名是cloudflare注册的, 自定义域后会自动的在cloudflare网站的DNS下面解析, 如果你的域名在其他服务商, 可能需要自己去做解析
![6.2](https://missuo.ru/file/be0be9538ffc4df9f2f0f.png)


### 遇到的问题

在cloudflare部署成功后, 我发现网页能打开,但是看不到文章, 查找了半天才明白, 需要将在创建博客文章的时候, 自动生成的toml格式内容的`draft`设置为false 

![7](https://missuo.ru/file/f48a0b7e6232a18d14efc.png)


### 其他
- [我的掘金](https://juejin.cn/user/2647279733058408)
- 我的公众号
![我的公众号](https://missuo.ru/file/a31692d797eec928eacc3.jpg)