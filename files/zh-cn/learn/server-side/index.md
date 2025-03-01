---
title: 服务器端网页编程
slug: Learn/Server-side
---

{{LearnSidebar}}

**_动态网页——服务器端编程_**这主题是一系列的模块来演示如何创建动态的网页；可以交付自定义的信息来回应 HTTP 请求的网页。这些模块为服务器端编程提供了一个通用的介绍，以及如何使用 Django (Python) 和 Express (Node.js/JavaScript) 去创建基础应用的具体的入门指导。

大多数的主流网页使用一类服务器端的技术去动态地显示所要求的不同数据。举个例子，想象一下 Amazon 上有多少可购买的产品，以及 FaceBook 上有多少帖子？用完全不同的静态页面去显示所有的这些内容会彻底地低效，所以取而代之的是这些网站展示的是静态的模板（用 [HTML](/zh-CN/docs/Learn/HTML), [CSS](/zh-CN/docs/Learn/CSS), 和 [JavaScript](/zh-CN/docs/Learn/JavaScript) 构建），然后在有需要时动态地在这些模板中更新数据展示，比如说当你想要在 Amazon 上浏览一个不同的产品。

在现代的网页开发世界里，学习服务器端开发是高度推荐的。

## 学习路径

开始服务器端编程通常比客户端编程要简单，因为动态的页面倾向于执行非常类似的操作（从数据库中获取数据然后显示到一个页面中，确认用户输入的数据以及保存到一个数据库中，检查用户的权限和登陆用户，以及更多），并且它是用能使这些和其他的常见网页服务端操作变简单的网页框架来构建的。

知道一些关于编程概念（或者关于一个特定的编程语言）的基础知识会很实用，但不是必要的。类似的，精通客户端编程也不是必修的，但一些基本知识会帮助你和创建你的客户端的“前端”开发者更融洽地工作。

你会需要去理解“网页是如何工作的”。我们推荐你先去阅读以下主题：

- 什么是一个网页服务器 \[[What is a web server](/zh-CN/docs/Learn/Common_questions/What_is_a_web_server)]
- 我需要什么软件去构建一个网页？ \[[What software do I need to build a website?](/zh-CN/docs/Learn/Common_questions/What_software_do_I_need)]
- 你怎样上传文件到一个网页服务器？ \[[How do you upload files to a web server?](/zh-CN/docs/Learn/Common_questions/Upload_files_to_a_web_server)]

拥有这些基础理解，你会做好完成在这节中的模块的准备。

## 模块

这个主题包含了以下的模块。你应该从第一个模块开始，然后接着到后面的任一模块，后面的模块演示了如何使用两个应用了合适的网页框架的非常流行的服务器端语言。

- 服务器端编程的第一步 \[[Server-side website programming first steps](/zh-CN/docs/Learn/Server-side/First_steps)]
  - : 这个模块提供了关于服务器端网页编程的服务器技术无关的信息 \[server-technology-agnostic information]，包括了关于服务器端编程的根本问题的答案——”它是什么“，”它跟客户端编程的区别“，和”为什么它很实用“——以及关于一些流行的服务器端框架的概述和如何为你的网站选择最合适的框架的指南。最后我们提供了一个关于网页服务器安全的介绍性部分。
- Django 网页框架 \[[Django Web Framework (Python)](/zh-CN/docs/Learn/Server-side/Django)]
  - : Django 是一个非常流行以及功能齐全的服务器端网页框架，它是用 Python 编写的。这个模块讲解了为什么 Django 是一个这么好的网页服务器框架，如何设立一个开发环境以及如何使用它来执行常见的任务。
- Express 网页框架 \[[Express Web Framework (Node.js/JavaScript)](/zh-CN/docs/Learn/Server-side/Express_Nodejs)]
  - : Express 是用 JavaScript 编写并在 node.js 运行时环境中托管的一个流行的网页框架。这个模块讲解了这个框架的一些主要优点，如何设立你的开发环境以及如何执行常见的网页开发和部署的任务。
