# Doragd's Blog and Wiki.:paw_prints: 

利用Travis CI + MkDocs + Github Pages 实现博文自动部署

## 坎坷的选择:x:

* `doragd.github.io/doragd-wiki` 
  * 好处：这种方式可以指定发布pages的分支为`gh-pages` 
  * 好处：可以用`mkdocs gh-deploy` 直接实现
  * 缺点：仓库名大小写敏感，而且`doragd.github.io` 是`404`
* `doragd.github.io`  
  * 好处：直接是主页
  * 坏处：限定了发布pages的分支是`master`
  * 坏处：必须用`mkdocs gh-deploy -b master` 实现，这样上传上去的文件只有`sites`下面的静态文件，无法用到`Travis CI` 来监控`docs`更新实现持续集成

* 最终方案:strawberry: 
  * 利用`Notes` 保存源文件，然后监控`Notes`的变动，部署到`doragd.github.io`
  * 坏处：需要用两个仓库来实现
  * 补充：使用VSCode编辑markdown并推送到`Notes`上
    * 先暂存所有修改，然后提交暂存文件，然后先拉取再推送
    *  `Ctrl+shift+G` 中实际只需输入消息，点击"√"的按钮即可commit，然后先拉取再推送
    * 注意：clone仓库的时候注意使用ssh来克隆，从而可以使用无密码的ssh key

## 配置过程:anger:

* `pip install` 记得加`--user ` （本地的话，否则可能出现没权限情况）
  * 最好以后还是加个虚拟环境
*  设置ssh key后，为了实现无密码登录，要将https替换为ssh方式

* VSCode支持：`Git` + `Markdown All In One` + `Markdown Extended` (支持Mkdocs的特有语法)

## 非常有用的参考资料:kissing_heart: 

* [Github + Travis + Mkdocs 搭建文档库](https://flc.io/more/github-travis-mkdocs-document/)
* [利用Travis CI、MkDocs自動部署Blog至GitHub Pages](https://cuiqingwei.github.io/2016/10/27/2016-10-27-%E5%88%A9%E7%94%A8Travis-CI%E3%80%81MkDocs%E8%87%AA%E5%8B%95%E9%83%A8%E7%BD%B2Blog%E8%87%B3GitHub-Pages/)
* [使用 Travis CI 自动更新 GitHub Pages]( https://neveryu.github.io/2019/02/05/travis-ci/ )
* [OI-Wiki](https://github.com/OI-wiki/OI-wiki/)
* [MkDocs](https://www.mkdocs.org/)
* [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/getting-started/)



 

