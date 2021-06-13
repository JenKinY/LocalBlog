### 一、编写一个简单 Scrapy 项目步骤：

- 新建项目（scrapy startproject xxx）：新建一个爬虫项目
- 明确目标（编写 item.py）：明确想要抓取的目标
- 制作爬虫（spiders/xxspider.py）：制作爬虫开始爬取网页
- 存储内容（pipelines.py）：设计管道存储爬取内容

### 二、常用命令说明

在命令行输入 `scrapy`后，会有如下输出，其中列举了 scrapy 的一些常用命令：

```bash
(venv) E:\SkyzcDev\PycharmProject\ScrapyLearn\tutorila>scrapy
Scrapy 1.8.0 - project: tutorila

Usage:
  scrapy <command> [options] [args]

Available commands:
  bench         Run quick benchmark test
  check         Check spider contracts
  crawl         Run a spider
  edit          Edit spider
  fetch         Fetch a URL using the Scrapy downloader
  genspider     Generate new spider using pre-defined templates
  list          List available spiders
  parse         Parse URL (using its spider) and print the results
  runspider     Run a self-contained spider (without creating a project)
  settings      Get settings values
  shell         Interactive scraping console
  startproject  Create new project
  version       Print Scrapy version
  view          Open URL in browser, as seen by Scrapy

Use "scrapy <command> -h" to see more info about a command

```

- bench 测试爬虫
- fetch 可直接查看响应
- genspider 创建爬虫
- runspider 启动爬虫
- shell 启动 shell 爬虫
- settings 配置环境
- startproject 创建项目
- version 查看版本号
- view 浏览器视图

全局命令:

- [`startproject`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-startproject)
- [`settings`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-settings)
- [`runspider`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-runspider)
- [`shell`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-shell)
- [`fetch`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-fetch)
- [`view`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-view)
- [`version`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-version)

项目(Project-only)命令:

- [`crawl`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-crawl)
- [`check`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-check)
- [`list`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-list)
- [`edit`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-edit)
- [`parse`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-parse)
- [`genspider`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-genspider)
- [`deploy`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-deploy)
- [`bench`](https://scrapy-chs.readthedocs.io/zh_CN/latest/topics/commands.html#std:command-bench)