# CheverJohn's website

[![Chinese version](http://cdn.mr8god.cn/img/chinese.svg)](README-zh.md)[![English version](https://cdn.mr8god.cn/img/english.svg)](README.md)
You can click on the badges to jump to the different language versions of the README file.

## Background

Those familiar with me know that I have many personal websites. Still, the longest one is the last one I built with docusaurus (<https://www.cheverjohn.xyz/>), which lasted for more than six months. I think I still like to output some textual things. I want to share what I've learned, keep communicating with others, and learn about miscellaneous items that are not limited to technology. So this is the **initial idea** for creating my website.

In addition, to build a personal website, I went through roughly this technical route.

| Timeline | Domains | Tech stack | Descriptions |
| ------- | --------- | ------ | -------- |
| 2020.02-2020.06 | mr8god.cn | hexo/Travis ci/GitHub action  | Writing some technical articles and understanding the importance of tools such as CI. |
| 2020.07 ～ 2021.06 | mr8god.cn | Python / Django / Supervisor / CICD / Vue | This is a very holistic project, I did **front and back-end separation**, **CICD**, **blog community system,** and many other interesting things. |
| 2021.10 ～ 2022.06 | cheverjohn.xyz | Docusaurus / React /vercel | This is an open source project from **meta** **Docusaurus**, generally used by the open source community as a little more documentation site, personal use for blogging I also think quite enough, but I still like to define their own format a little more, even if I have a different format for each blog. |
| 2022.07.17 ～ now | cheverjohn.me | Html / nginx / js / query | Get ready for the big game! |

Anyway, I have high hopes for the latest blog, someone always needs to toss a little bah~

## TODO

### Cloud-Native

- [ ] Docker Deployment: Packaging the front-end project and deploying it as a Docker image.
- [ ] Try cluster deployment based on the completion of the front-end tasks.

#### API Gateway Selection

- [ ] Explore the feasibility of and need for API gateways.

### Font-end

- [ ] Unify the html style of the blog.
- [ ] Go ~~ pit ~~ Kun Kun, let Kun Kun help me to get the front-end style architecture.

### O&M

#### CI/CD

- [ ] Get the auto-deployment right, with the specific requirement that my deployment server will automatically pull the code as soon as I submit the local code;
  - [ ] Conduct CI tool selection;
  - [ ] Implemented on a cloud-based server.

#### Grayscale Release / Canary Release

- [ ] As the subheading, to do a good **grayscale release** with nginx.

## Deployment Method

### Buy A Domain

Just find a suitable domain name provider and buy it. Here we are looking for namesilo.

Then note that we just need to change the nameserver of namesilo. Change it to the following.

```yaml
ns1.vultr.com
ns2.vultr.com
```

These two domains are available on the server provider's side.

### Buy A Server

Find a reliable server provider and get a server. The one I'm looking for here is vultr.

After you buy the server, you need to pay attention to the fact that you can just use the DNS resolution service that comes with it and build it.

### NGINX Configuration

Use **NGINX** as a reverse proxy server to reverse proxy the route to index.html and that's it. You can refer to this [link](https://www.vultr.com/zh/docs/how-to-install-and-configure-nginx-on-a-vultr-cloud-server/#:~:text=Encrypt%20guide%20here.-,Configure%20Nginx%20as%20a%20Reverse%20Proxy,-Nginx%20can%20work)for more information.

Run the following command to view my configuration.

```bash
cat /etc/nginx/conf.d/cheverjohn.me.conf
```

The configuration of my **NGINX** is as follows.

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
