# 江某人的个人网站

[![Chinese version](http://cdn.mr8god.cn/img/chinese.svg)](README-zh.md)  [![English version](https://cdn.mr8god.cn/img/english.svg)](README.md)

你可以通过点击这个图标来切换不同语言版本的 README.md 文件。

至于为什么要自称江某人呢？这得从一个高中数学老师说起……

好了好了，咱不扯那么远～回到今天的话题——搭建个人网站。我们从背景讲起叭～

## 背景

熟悉我的人知道，我有过很多个个人网站，坚持最长的估计就是上一任，用 docusaurus 搭建的网站了（<https://www.cheverjohn.xyz/>）了，整整坚持了六个月有余。我自认为还是很喜欢输出一些文字性的东西的。喜欢将自己所学习的东西分享出去，不断与他人交流，了解一些杂七杂八的东西，不仅限于技术。所以这是我有个人网站的**本心**。

此外，搭建个人网站，我经历了大概这样的技术路线。

| 时间  | 域名    | 技术栈 | 大致作用 |
| ------- | --------- | ------ | -------- |
| 2020.02-2020.06 | mr8god.cn | hexo/travi ci/GitHub action   | 写一些技术文章，理解了 CI 等工具的重要性。 |
| 2020.07 ～ 2021.06 | mr8god.cn | Python / Django / Supervisor / CICD / Vue | 这就是一个非常具有整体性的项目了，我做到了**前后端分离**、**CICD**、**博客社区系统**等很多有意思的事情。 |
| 2021.10 ～ 2022.06 | cheverjohn.xyz | Docusaurus / React /vercel | 这是一个来自于 **meta** 的开源项目 **Docusaurus**，一般开源社区用作文档网站比较多一点，个人拿来做博客我也觉得相当够用了，不过我还是喜欢自己定义格式更多一点，哪怕我每一篇博客的格式都不一样。 |
| 2022.07.17 ～ now | cheverjohn.me | Html / nginx / js / query | 准备大干一场！ |

总之，我对最新的博客寄予厚望，总是需要有人折腾一点的叭～

## 部署方式

### 购买域名

找到一个合适的域名提供商购买即可。这边找的是 namesilo。

然后需要注意的是，我们只需要更改 namesilo 的 nameserver 即可。将其更改为如下：

```yaml
ns1.vultr.com
ns2.vultr.com
```

这两个域名可以在服务器提供商那边获得。

### 购买服务器

找一个靠谱的服务器提供商，获得一个服务器。这边找的是 vultr。

买了服务器之后，需要注意的是，直接使用自带的 DNS 解析服务，构建一下即可。

### NGINX 配置

使用 **NGINX** 作为反向代理服务器，将路由反向代理到 index.html，仅此而已。可以参考这个[链接](https://www.vultr.com/zh/docs/how-to-install-and-configure-nginx-on-a-vultr-cloud-server/#:~:text=Encrypt%20guide%20here.-,Configure%20Nginx%20as%20a%20Reverse%20Proxy,-Nginx%20can%20work)，获取更多的信息。

运行如下命令查看我的配置：

```bash
cat /etc/nginx/conf.d/cheverjohn.me.conf
```

我的 **NGINX** 的配置如下：

```nginx
server {
    listen 80;
    listen [::]:80;

    server_name cheverjohn.me www.cheverjohn.me;

    root /home/vultrchever/website/static;

    index index.html;

    access_log /var/log/nginx/cheverjohn.me.access.log;
    error_log /var/log/nginx/cheverjohn.me.error.log;

    location / {
        try_files $uri $uri/ =404;
    }

}
```

### API 网关部署

#### 编写个人网站的 Dockerfile

Dockerfile 文件内容如下：

```dockerfile
FROM nginx:alpine
COPY ./static /usr/share/nginx/html
```

#### 将个人网站打包成镜像

打包镜像命令如下：

```bash
docker build -t html-server-image:v1 .
```

然后运行命令如下：

```bash
docker run -d -p 80:80 html-server-image:v1
```

#### 安装 APISIX

直接根据官网的 Docker compose 方式安装即可。

## TODO

### 云原生

- [ ] Docker 部署：将前端项目打包好，以 Docker 镜像的方式进行部署；
- [ ] 在前置任务完成的基础上，尝试集群部署。

#### API 网关选型

- [ ] 探索 API 网关的可行性以及必要性。

### 前端

- [ ] 将博客的 html 样式统一；
- [ ] 去~~坑~~坤坤，让坤坤帮俺搞好前端的样式架构。

### 运维

#### CICD

- [ ] 将自动部署搞好，具体需求：我一提交本地代码，我的部署服务器便会自动拉取代码
  - [ ] 进行 CI 工具选型；
  - [ ] 在云端服务器上实现。

#### 灰度发布/金丝雀发布

- [ ] 如小标题，要用 nginx 做好**灰度发布**

