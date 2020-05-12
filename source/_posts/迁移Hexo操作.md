有时候我们可能要在另一台电脑上使用我们的博客平台，这时，需要我们重新再操作一个远程仓库，然后在创建文章吗？？？   
很明显，这是不现实的，我们有什么办法解决这个问题呢？，有的，我们可以做一个远程仓库备份就可以了，这样我们下次使用只要配置好环境，直接clone下来就可以使用了，来，先看步骤！

1. 在GitHub上创建远程分支（都是点点点的操作）

2. 配置环境
    - Git
    - Node.js
    - Hexo
        - `sudo npm install hexo-cli -g`

3. `clone`仓库（这里有好几步）
    - 克隆下来后，再创建一下分支`git checkout -b dev`,
    - 删除掉`.git`以外的文件，务必要这样做。注意：不是删除掉`.git`。
    - 然后将`Blog`文件夹下的所有文件移到这个目录，并使用`git`操作一下，操作如下：
        ```
        git add .
        git commit -m ''
        git push --set-upstream origin dev
        ```
    - 完成到这里就结束了这些部分的操作
    - 然后这样一下`npm install` （可能不需要）

3. 开始正常写文章
    - 重复[Hexo配合GitHub搭建个人博客.md](Hexo配合GitHub搭建个人博客.md)里面的5～6步骤就可以了。

4. 最后一步
    - 写完之后，得同步到dev分支一下奥
        ```
        git add .
        git commit -m ''
        git push origin dev
        ```