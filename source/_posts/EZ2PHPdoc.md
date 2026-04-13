---
abbrlink: ''
categories: []
comments: true
date: '2026-04-13T23:05:53.580924+08:00'
donate: true
excerpt: 🚀 EZ2PHP  An SUPER simple PHP Framework. &gt; 专为“懒人”设计的轻量级 MVC 解决方案，拒绝反复复制粘贴。  ---（本文档由AI优化过） 📖 简介 EZ2PHP 正如其名（Easy to PHP），是为了在处理中小型项目时，既能享受 MVC 模式的清晰结构，又不愿被庞大框架的复杂度困扰而诞生的。 ✨ 项目特性  极致轻量：仅包含目录结构、视图渲...
share: true
tags: []
title: title
toc: true
updated: '2026-04-13T23:10:42.048+08:00'
---
# 🚀 EZ2PHP

> **An SUPER simple PHP Framework.** > 专为“懒人”设计的轻量级 MVC 解决方案，拒绝反复复制粘贴。

---（本文档由AI优化过）

## 📖 简介

**EZ2PHP** 正如其名（Easy to PHP），是为了在处理中小型项目时，既能享受 MVC 模式的清晰结构，又不愿被庞大框架的复杂度困扰而诞生的。

### ✨ 项目特性

* **极致轻量**：仅包含目录结构、视图渲染和数据库控制，核心极简。
* **高扩展性**：采用原生 PHP 逻辑，方便根据需求进行二次开发。
* **极简部署**：只需简单的 Nginx 伪静态配置即可运行。
* **简洁语法**：支持基础的模板变量替换与循环。

---

## 🛠️ 开始使用

### 1. 安装与部署

你可以通过以下两种方式获取源码：

* **方式一**：使用 Git Clone 到本地。
  ```bash
  git clone [https://github.com/ElythraDev/EZ2PHP.git](https://github.com/ElythraDev/EZ2PHP.git)
  ```
* **方式二**：直接下载源码解压至 Web 目录。

### 2. 环境配置

在 `config.php` 内，你可以配置环境类型及数据库连接信息：

```php
// 设置环境类型: 'development' (开发) 或 'production' (生产)
if (!defined('ENVIRONMENT')) define('ENVIRONMENT', 'development');
```

### 3. 服务器部署 (Nginx)

为了让路由正常工作，需要将所有请求重写到 `public/index.php`。以下是参考配置：

```nginx
server {
    listen 80;
    root "/your/site/root/public";
    index index.php;

    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    # 保护核心目录不被直接访问
    location ~ ^/(lib|pages|templates) {
        deny all;
    }
}
```

---

## 📂 了解目录结构

EZ2PHP 的结构非常直观：

| 目录/文件 | 描述 |
| :--- | :--- |
| `/lib` | 存储核心 LIB 文件（如视图类等） |
| `/pages` | **控制器**：处理具体的页面业务逻辑 |
| `/public` | 访客入口及静态资源（JS/CSS/IMG）存放地 |
| `/templates` | **视图模板**：HTML 结构文件 |
| `router.php` | 路由配置文件 |
| `config.php` | 全局配置文件 |

> 💡 **提示**：`/pages` 默认缺省 `404.php`，你可以参照 `sample.php` 快速编写一个。

---

## 🚦 路由 (Routing)

在 `router.php` 中注册你的路由规则。目前主要支持 GET 请求。

### 静态路由

```php
$router->get('/', function() {
    require __DIR__ . '/pages/sample.php';
});
```

### 动态参数路由

```php
// 示例：匹配 /article/arc101.html
$router->get('/article/arc{id}.html', function($params) {
    $id = (int)$params['id']; // 获取路径中的参数
    require __DIR__ . '/pages/articleDisplay.php';
});
```

---

## 🎮 控制器 (Controller)

以 `/pages/sample.php` 为例，这是一个典型的控制器实现：

```php
<?php
require __DIR__ . '/../config.php';   // 数据库连接
require __DIR__ . '/../lib/view.php'; // 视图渲染引擎

// 业务逻辑处理并渲染模板
$content = View::render(__DIR__ . '/../templates/sample.php', [
    'title' => 'EZ2PHP OKKKKKKKK!',
]);

echo $content;
?>
```

---

## 🎨 模板引擎 (Templates)

EZ2PHP 支持基础的变量替换和循环输出。

### 变量输出

在模板文件中使用 `{变量名}` 进行输出：

```html
<h1>{title}</h1>
<p>Hello World! EZ2PHP is running.</p>
```

### 循环输出

支持与 PHP 语法高度相似的 foreach 循环：

```html
{foreach items as item}
    <div class="card">
        <h3>{item.title}</h3>
        <p>{item.content}</p>
    </div>
{/foreach}
```

目前还没有像Smarty那样支持模板内的运算。

---

## 🗄️ 数据库操作

推荐使用 PDO 预处理方式以保证安全性：

```php
$sql = 'SELECT * FROM article ORDER BY id DESC LIMIT ?, ?';
$stmt = $pdo->prepare($sql);
$stmt->execute([$offset, $itemsPerPage]);

$posts = $stmt->fetchAll(PDO::FETCH_ASSOC);
```

---

## 🤝 贡献与反馈

Code with love. 如果你在使用中遇到任何问题，欢迎通过 GitHub Issues 提出。

---

**Maintained by [Elythra](https://github.com/ElythraDev)**

