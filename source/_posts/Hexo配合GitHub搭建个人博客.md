### Hexo配合GitHub搭建个人博客

1. 安装各类软件
    - Git
        - emmmm  这个自己去百度去
    - Node.js
        - 这个也百度去
    - Hexo
        - `sudo npm install hexo-cli -g`

2. 初始化   
    - 找个自己喜欢的目录，创建一个喜欢的目录名
        - eg: `mkdir Blog`
    - 进入目录并初始化
        - `cd Blog`
        - `hexo init`
        - `npm install`

3. 运行
    - `hexo server`

4. 进行部署
    - 进入GitHub创建一个公共仓库，取名为`<GitHub账户名>.github.io`
    - 打开`Blog`目录下的`_config.yml`，修改一下最后几行
    - eg：   
        ```
        deploy:
        type: git
        repo: 自己的仓库地址
        branch: master
        ```
5. 创建文章
    - 在`Blog/source/_posts/`目录下开始写吧
    - 想要插入图片？ 没问题
        - 当前目录下创建一个文件夹，放置图片，他不香吗？
        - eg:   
            ```
            .
            ├── img
            │   └── 20200512
            │       └── 828879970.png
            └── _posts
                └── Hexo配合GitHub搭建个人博客.md
            ```
    - 图片演示   
        ![alt](../img/20200512/828879970.png)
    
6. 上传
    - `hexo clean`
    - `hexo generate` 或者 `hexo g`
    - `hexo deploy` 或者 `hexo d`

7. 关于换主题，emmm，我觉得自带的很好看哇，就不换了。