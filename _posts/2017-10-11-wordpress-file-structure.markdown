---
layout: post
title:  "WordPress文件结构与数据库结构笔记"
categories: Programmer
tags: WordPress
author: 西夏
description: WordPress学习笔记一，WordPress文件目录结构与数据库结构说明。
---
### 1，WordPress文件目录结构整理
{% highlight shell %}
   root\
    |
    |-wp-admin\             # 后台管理需要的类和文件
    |
    |-wp-includes\          # WordPress强大的类库，及核心函数定义
    |  |
    |  |-class-*.php        # WordPress定义的类
    |  |-post-template.php  
    |  |  |-the_ID()
    |  |  |-the_title()
    |  |           
    |  |-wp-db.php
    |  |  |-query()
    |  |  |-insert()
    |  |  |-update()
    |
    |-wp-content\            # 主题开发主要就是修改这个目录下的文件，其他文件不要动！
    |  |
    |  |-languages\          # 多语言包，使用Poedit编辑和创建语言包
    |  |-plugins\            # 插件安装目录
    |  |-themes\             # 主题安装目录
    |  |-uploads\            # 文件上传目录
    |  |-upgrade\
    |  |
    |  |-index.php       
    |
    |-index.php              # 核心索引文件，博客输出文件
    |-license.txt            # GPL许可证
    |-readme.html            # 安装说明
    |-wp-activate.php        # 用户注册激活
    |-wp-blog-header.php     # 加载WordPress环境和模板
    |-wp-comments-post.php   # 评论相关操作
    |-wp-config.php          # 链接MySQL数据库的配置文件，安装完WordPress后自动生成
    |-wp-config-sample.php   # WordPress官方给出的连接MySQL数据库的配置示例
    |-wp-cron.php            # 执行定时任务
    |-wp-links-opml.php      # 生成OPML格式的链接列表，通过WordPress的管理菜单添加
    |-wp-load.php            # 加载wp-config.php和设置公共变量，加载WordPress程序和类库
    |-wp-login.php           # 定义注册用户登陆界面
    |-wp-mail.php            # 邮件操作
    |-wp-settings.php        # WordPress运行前例行程序，检查安装是否成功，加载辅助函数，用户插件初始化等
    |-wp-signup.php          # 用户注册，WordPress多站点功能
    |-wp-trackback.php       # 处理trackback请求
    |-xmlrpc.php             # 远程发布功能
    |
{% endhighlight %}


<br/>
### 2，WordPress数据库结构整理
#### 2.1, WordPress默认创建的12张表

{% highlight shell %}
mysql> use wordpress_;
Database changed
mysql> show tables;
+----------------------------+
| Tables_in_wordpress_       |
+----------------------------+
| wp_commentmeta             |  // 评论信息额外补充表
| wp_comments                |  // 评论信息表
| wp_links                   |  // 链接信息表
| wp_options                 |  // 基本信息表
| wp_postmeta                |  // 文章信息额外补充表
| wp_posts                   |  // 文章信息表
| wp_term_relationships      |  // 分类与文章(category,post_tag)，链接(link_category)，子菜单项(nav_menu)的关系
| wp_term_taxonomy           |  // 分类类型，category,link_category,post_tag,nav_menu
| wp_termmeta                |  // 分类信息额外补充表
| wp_terms                   |  // 分类信息表
| wp_usermeta                |  // 用户信息额外补充表
| wp_users                   |  // 用户信息表
+----------------------------+
{% endhighlight %}

<br/>
#### 2.2，WordPress表详解
#### 2.2.1，评论信息
{% highlight shell %}
mysql> describe wp_commentmeta;         // 评论信息额外补充表
+------------+---------------------+------+-----+---------+----------------+
| Field      | Type                | Null | Key | Default | Extra          |
+------------+---------------------+------+-----+---------+----------------+
| meta_id    | bigint(20) unsigned | NO   | PRI | NULL    | auto_increment |
| comment_id | bigint(20) unsigned | NO   | MUL | 0       |                |
| meta_key   | varchar(255)        | YES  | MUL | NULL    |                |
| meta_value | longtext            | YES  |     | NULL    |                |
+------------+---------------------+------+-----+---------+----------------+

mysql> describe wp_comments;            // 评论信息表
+----------------------+---------------------+------+-----+---------------------+----------------+
| Field                | Type                | Null | Key | Default             | Extra          |
+----------------------+---------------------+------+-----+---------------------+----------------+
| comment_ID           | bigint(20) unsigned | NO   | PRI | NULL                | auto_increment |
| comment_post_ID      | bigint(20) unsigned | NO   | MUL | 0                   |                |
| comment_author       | tinytext            | NO   |     | NULL                |                |
| comment_author_email | varchar(100)        | NO   | MUL |                     |                |
| comment_author_url   | varchar(200)        | NO   |     |                     |                |
| comment_author_IP    | varchar(100)        | NO   |     |                     |                |
| comment_date         | datetime            | NO   |     | 0000-00-00 00:00:00 |                |
| comment_date_gmt     | datetime            | NO   | MUL | 0000-00-00 00:00:00 |                |
| comment_content      | text                | NO   |     | NULL                |                |
| comment_karma        | int(11)             | NO   |     | 0                   |                |
| comment_approved     | varchar(20)         | NO   | MUL | 1                   |                |
| comment_agent        | varchar(255)        | NO   |     |                     |                |
| comment_type         | varchar(20)         | NO   |     |                     |                |
| comment_parent       | bigint(20) unsigned | NO   | MUL | 0                   |                |
| user_id              | bigint(20) unsigned | NO   |     | 0                   |                |
+----------------------+---------------------+------+-----+---------------------+----------------+
{% endhighlight %}


<br/>
#### 2.2.2，链接信息
{% highlight shell %}
mysql> describe wp_links;               // 链接信息表
+------------------+---------------------+------+-----+---------------------+----------------+
| Field            | Type                | Null | Key | Default             | Extra          |
+------------------+---------------------+------+-----+---------------------+----------------+
| link_id          | bigint(20) unsigned | NO   | PRI | NULL                | auto_increment |
| link_url         | varchar(255)        | NO   |     |                     |                |
| link_name        | varchar(255)        | NO   |     |                     |                |
| link_image       | varchar(255)        | NO   |     |                     |                |
| link_target      | varchar(25)         | NO   |     |                     |                |
| link_description | varchar(255)        | NO   |     |                     |                |
| link_visible     | varchar(20)         | NO   | MUL | Y                   |                |
| link_owner       | bigint(20) unsigned | NO   |     | 1                   |                |
| link_rating      | int(11)             | NO   |     | 0                   |                |
| link_updated     | datetime            | NO   |     | 0000-00-00 00:00:00 |                |
| link_rel         | varchar(255)        | NO   |     |                     |                |
| link_notes       | mediumtext          | NO   |     | NULL                |                |
| link_rss         | varchar(255)        | NO   |     |                     |                |
+------------------+---------------------+------+-----+---------------------+----------------+
{% endhighlight %}


<br/>
#### 2.2.3，基本信息
{% highlight shell %}
mysql> describe wp_options;             // 基本信息存储表，该表常用于插件数据存储
+--------------+---------------------+------+-----+---------+----------------+
| Field        | Type                | Null | Key | Default | Extra          |
+--------------+---------------------+------+-----+---------+----------------+
| option_id    | bigint(20) unsigned | NO   | PRI | NULL    | auto_increment |
| option_name  | varchar(191)        | NO   | UNI |         |                |
| option_value | longtext            | NO   |     | NULL    |                |
| autoload     | varchar(20)         | NO   |     | yes     |                |
+--------------+---------------------+------+-----+---------+----------------+
{% endhighlight %}

<br/>
#### 2.2.4，文章信息
{% highlight shell %}
mysql> describe wp_postmeta;            // 文章信息额外补充表
+------------+---------------------+------+-----+---------+----------------+
| Field      | Type                | Null | Key | Default | Extra          |
+------------+---------------------+------+-----+---------+----------------+
| meta_id    | bigint(20) unsigned | NO   | PRI | NULL    | auto_increment |
| post_id    | bigint(20) unsigned | NO   | MUL | 0       |                |
| meta_key   | varchar(255)        | YES  | MUL | NULL    |                |
| meta_value | longtext            | YES  |     | NULL    |                |
+------------+---------------------+------+-----+---------+----------------+

mysql> describe wp_posts;               // 文章信息表
+-----------------------+---------------------+------+-----+---------------------+----------------+
| Field                 | Type                | Null | Key | Default             | Extra          |
+-----------------------+---------------------+------+-----+---------------------+----------------+
| ID                    | bigint(20) unsigned | NO   | PRI | NULL                | auto_increment |
| post_author           | bigint(20) unsigned | NO   | MUL | 0                   |                |
| post_date             | datetime            | NO   |     | 0000-00-00 00:00:00 |                |
| post_date_gmt         | datetime            | NO   |     | 0000-00-00 00:00:00 |                |
| post_content          | longtext            | NO   |     | NULL                |                |
| post_title            | text                | NO   |     | NULL                |                |
| post_excerpt          | text                | NO   |     | NULL                |                |
| post_status           | varchar(20)         | NO   |     | publish             |                |
| comment_status        | varchar(20)         | NO   |     | open                |                |
| ping_status           | varchar(20)         | NO   |     | open                |                |
| post_password         | varchar(255)        | NO   |     |                     |                |
| post_name             | varchar(200)        | NO   | MUL |                     |                |
| to_ping               | text                | NO   |     | NULL                |                |
| pinged                | text                | NO   |     | NULL                |                |
| post_modified         | datetime            | NO   |     | 0000-00-00 00:00:00 |                |
| post_modified_gmt     | datetime            | NO   |     | 0000-00-00 00:00:00 |                |
| post_content_filtered | longtext            | NO   |     | NULL                |                |
| post_parent           | bigint(20) unsigned | NO   | MUL | 0                   |                |
| guid                  | varchar(255)        | NO   |     |                     |                |
| menu_order            | int(11)             | NO   |     | 0                   |                |
| post_type             | varchar(20)         | NO   | MUL | post                |                |
| post_mime_type        | varchar(100)        | NO   |     |                     |                |
| comment_count         | bigint(20)          | NO   |     | 0                   |                |
+-----------------------+---------------------+------+-----+---------------------+----------------+
{% endhighlight %}

<br/>
#### 2.2.5，分类信息
{% highlight shell %}
mysql> describe wp_terms;               // 分类信息表
+------------+---------------------+------+-----+---------+----------------+
| Field      | Type                | Null | Key | Default | Extra          |
+------------+---------------------+------+-----+---------+----------------+
| term_id    | bigint(20) unsigned | NO   | PRI | NULL    | auto_increment |
| name       | varchar(200)        | NO   | MUL |         |                |
| slug       | varchar(200)        | NO   | MUL |         |                |
| term_group | bigint(10)          | NO   |     | 0       |                |
+------------+---------------------+------+-----+---------+----------------+

mysql> describe wp_termmeta;            // 分类信息额外补充表
+------------+---------------------+------+-----+---------+----------------+
| Field      | Type                | Null | Key | Default | Extra          |
+------------+---------------------+------+-----+---------+----------------+
| meta_id    | bigint(20) unsigned | NO   | PRI | NULL    | auto_increment |
| term_id    | bigint(20) unsigned | NO   | MUL | 0       |                |
| meta_key   | varchar(255)        | YES  | MUL | NULL    |                |
| meta_value | longtext            | YES  |     | NULL    |                |
+------------+---------------------+------+-----+---------+----------------+

mysql> describe wp_term_relationships;  // 分类与文章，链接，子菜单的关联信息表
+------------------+---------------------+------+-----+---------+-------+
| Field            | Type                | Null | Key | Default | Extra |
+------------------+---------------------+------+-----+---------+-------+
| object_id        | bigint(20) unsigned | NO   | PRI | 0       |       |
| term_taxonomy_id | bigint(20) unsigned | NO   | PRI | 0       |       |
| term_order       | int(11)             | NO   |     | 0       |       |
+------------------+---------------------+------+-----+---------+-------+

mysql> describe wp_term_taxonomy;       // 分类补充信息表，区分terms的类型，
                                        // 有category、link_category、post_tag和nav_menu四种
+------------------+---------------------+------+-----+---------+----------------+
| Field            | Type                | Null | Key | Default | Extra          |
+------------------+---------------------+------+-----+---------+----------------+
| term_taxonomy_id | bigint(20) unsigned | NO   | PRI | NULL    | auto_increment |
| term_id          | bigint(20) unsigned | NO   | MUL | 0       |                |
| taxonomy         | varchar(32)         | NO   | MUL |         |                |
| description      | longtext            | NO   |     | NULL    |                |
| parent           | bigint(20) unsigned | NO   |     | 0       |                |
| count            | bigint(20)          | NO   |     | 0       |                |
+------------------+---------------------+------+-----+---------+----------------+
{% endhighlight %}


<br/>
#### 2.2.6，用户信息
{% highlight shell %}
mysql> describe wp_users;               // 用户信息表
+---------------------+---------------------+------+-----+---------------------+----------------+
| Field               | Type                | Null | Key | Default             | Extra          |
+---------------------+---------------------+------+-----+---------------------+----------------+
| ID                  | bigint(20) unsigned | NO   | PRI | NULL                | auto_increment |
| user_login          | varchar(60)         | NO   | MUL |                     |                |
| user_pass           | varchar(255)        | NO   |     |                     |                |
| user_nicename       | varchar(50)         | NO   | MUL |                     |                |
| user_email          | varchar(100)        | NO   | MUL |                     |                |
| user_url            | varchar(100)        | NO   |     |                     |                |
| user_registered     | datetime            | NO   |     | 0000-00-00 00:00:00 |                |
| user_activation_key | varchar(255)        | NO   |     |                     |                |
| user_status         | int(11)             | NO   |     | 0                   |                |
| display_name        | varchar(250)        | NO   |     |                     |                |
+---------------------+---------------------+------+-----+---------------------+----------------+

mysql> describe wp_usermeta;            // 用户信息额外补充表
+------------+---------------------+------+-----+---------+----------------+
| Field      | Type                | Null | Key | Default | Extra          |
+------------+---------------------+------+-----+---------+----------------+
| umeta_id   | bigint(20) unsigned | NO   | PRI | NULL    | auto_increment |
| user_id    | bigint(20) unsigned | NO   | MUL | 0       |                |
| meta_key   | varchar(255)        | YES  | MUL | NULL    |                |
| meta_value | longtext            | YES  |     | NULL    |                |
+------------+---------------------+------+-----+---------+----------------+
{% endhighlight %}

<!-- 后面是文章参考资料 -->
<br/>
### 特别鸣谢：

1，[《跟黄聪学WordPress主题开发》][hc]

<!-- 文章插图和超链接 -->
[hc]: http://wphun.com/673
