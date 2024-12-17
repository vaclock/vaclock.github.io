## 使用

> 前置依赖：在使用本主题之前，需要安装 Node.js, Ruby，以及 Bundle。详见[使用 Jekyll 创建 GitHub Pages 站点](https://docs.github.com/zh/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)


克隆并安装依赖

```bash
git clone git@github.com:vaclock/vaclock.github.io.git
bundle install
```

更改配置，尤其是移除 `ga`（Google Analytics）和 `disqus`（Disqus 评论），或替换为你自己的账号。

```bash
vim _config.yml
```

执行 `npm start` 启动，即可访问： <http:/localhost:4000>

## 致谢
Theme by <a href="https://harttle.land">Harttle Land</a>.

## 参考文档
- [jekyll docs](https://jekyllrb.com/docs/)
