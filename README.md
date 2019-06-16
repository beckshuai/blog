# 本地写文章
```
hexo server --draft
hexo new draft "file-name" // 新建文章到草稿
hexo publish "file-name" // 将草稿文章移到发布 
hexo deploy -g // 部署
```

如果有图片请放到source/imgs/目录下，并尽量按照post建子目录

文章里要显示图片，使用
```
![title](/imgs/xxxx/yyy.jpg)
```