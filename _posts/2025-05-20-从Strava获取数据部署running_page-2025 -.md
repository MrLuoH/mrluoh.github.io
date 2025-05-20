---
layout:     post
title:      从strava部署running_page
subtitle:   备忘
date:       2025-05-20
author:     Luo
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
    - Blog
---


1.克隆仓库 

> git clone https://github.com/yihong0618/running_page.git --depth=1

2.输入：

> cd running_page

3.依次运行3行命令；

> pip3 install -r requirements.txt
>
> yarn install
>
> yarn develop

4.进入running_page/assets/删除带年份的2012.....2023 svg文件

5.进入running_page/，将config-example.yaml复制并重命名config.yaml

6.拉取strava的数据（第一次同步 Strava 数据时需要更改在 strava_sync.py 中的第 12 行代码 False 改为 True，运行完成后，再改为 False）

> python run_page/strava_sync.py id client_secret refresh_token

refresh_token获取方法:

首先， 通过以下链接获取code,其中ID需要填写
>
> https://www.strava.com/oauth/authorize?client_id=&response_type=code&redirect_uri=http://localhost/exchange_token&approval_prompt=force&scope=activity:read_all
>
 其次，再用以下命令获取refresh_token

> curl -X POST https://www.strava.com/oauth/token \
-F client_id= \
-F client_secret= \
-F code= \
-F grant_type=authorization_code

7.生成自己的跑步数据，运行以下命令：
> python run_page/gen_svg.py --from-db --title "Mr’Luo Running" --type github --athlete "Luo" --special-distance 10 --special-distance2 20 --special-color yellow --special-color2 red --output assets/github.svg --use-localtime --min-distance 0.5

> python run_page/gen_svg.py --from-db --title “Running Route Map" --type grid --athlete “Mr’Luo"  --output assets/grid.svg --min-distance 10.0 --special-color yellow --special-color2 red --special-distance 20 --special-distance2 40 --use-localtim

> python run_page/gen_svg.py --from-db --type circular --use-localtime

> yarn develop

8.Github Desktop推送

9.在github/workflows/run_data_sync.yml修改名字/使用的APP/个人邮箱
> please change to your own config.
> 
> RUN_TYPE: strava # support strava/nike/garmin/garmin_cn/keep/....  
> 
> ATHLETE: Mr’Luo   TITLE: Mr’Luo Running

10.在仓库目录下找到 src/static/site-metadata.ts，找到以下内容并修改成你自己想要的。

siteTitle: 'Mr’Luo Running',
siteUrl: 'https://mrluoh.github.io/2023/11/07/%E9%83%A8%E7%BD%B2running_page-2023',
logo: 'https://s2.loli.net/2023/10/31/KFm1LUv3JIjTXdk.jpg',

description: 'Personal site and blog',
navLinks:
name: 'Blog',url: 'https://mrluoh.github.io',
name: 'About',
      url: 'https://mrluoh.github.io/about',

在running_page/src/utils/const.ts下，修改网页文字。

11.网页端配置strava自动更新：

setting ➡ secrets and variables

> STRAVA_CLIENT_ID：
>
> STRAVA_CLIENT_REFRESH_TOKEN：
>
>STRAVA_CLIENT_SECRET：
>
> TOKEN：

12.选择Github Actions

setting ➡ Pages
