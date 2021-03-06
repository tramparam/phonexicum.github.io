---
layout: page

title: _notes_

category: infosec
see_my_category_in_header: true

permalink: /infosec/unstructured_notes.html
show_thispage_in_header: false

published: true
---

<article class="markdown-body" markdown="1">

# Content
<div class="spoiler"><div class="spoiler-title">
    <i>Table of contents</i>
</div><div class="spoiler-text" markdown="1">

* TOC
{:toc}

</div>
</div>

<br>

## LFI -> RCE (by Log File Tainting)

Each users request leaves track on server side:

- storing media-files - ***file upload***
    - images
    - video (e.g. ffmpeg vulns, etc.)
- log records (`/apache/logs`, `/var/log/apache2`, `/proc/self/environ`, etc.)
- pseudo-protocols (`data://`, `php://`, `expect://`, etc.)
- tmp files

    `phpinfo ()` - files passed through http are stored by php into tmp files, tmp file-name can be guessed using information from phpinfo and using LFI it must be executed ([some expoit scripts examples](https://rdot.org/forum/showthread.php?t=1134&page=2))

    <br>
    tmp files lives until php-script will end its execution (actually cleanup will start before sending last chunk of data), ways to hold tmp file:

    - `Content-Length` must be falsy to hang php-script execution
    - network connection can be slowed down (e.g. small network/proxy packets, etc.) and `HTTP_Z` http header must be big to increase amount of data after `_FILES` variable in phpinfo output
    - load of script recursively including itself (php will die without cleaning tmp files)

- other places, where web-application stores data (e.g. sessions, e-mails, etc.)

Example of log file tainting with ruby: [Rails Dynamic Render to RCE (CVE-2016-0752)](https://nvisium.com/blog/2016/01/26/rails-dynamic-render-to-rce-cve-2016-0752/)

<!--Loading shell through LFI:

    Через медиа-файлы (фото, видео, документы и т. д.). Для реализации этого способа требуется доступ к странице загрузки файлов (возможно, админке или менеджеру файлов).
    Через файлы логов (/apache/logs/error.log, /var/log/access_log, /proc/self/environ, /proc/self/cmdline, /proc/self/fd/X и многие другие). Здесь стоит учесть, что чем больше размер логов, тем труднее произвести успешную атаку. В некоторых случаях PHP должен быть запущен в режиме совместимости с CGI или же должна существовать виртуальная файловая система /proc, для доступа к которой необходимы соответствующие права.
    Через псевдопротоколы (data:, php://input, php://filter), требующие наличия директивы allow_url_include=On (по умолчанию — Off) и версии PHP >= 5.2.
    Через файлы сессий (/tmp/sess_*, /var/lib/php/session/). Естественно, атакующий должен иметь возможность записывать свои данные в сессию.
    Через мыло. При этом в уязвимой CMS должна присутствовать возможность отправки писем от www-юзера, а также иметься доступная для чтения директория с отправленными мейлами (к примеру, /var/spool/mail).
    (/tmp/php*, C:tmpphp*). -->

<br>

---

## PHP

[PHP at the Core: A Hacker's Guide](http://php.net/manual/en/internals2.php)

Auto-typeconversion problems:

* [php magic hashes](https://www.whitehatsec.com/blog/magic-hashes/) - hashes that starts with `0e` and can be autoconverted by PHP to float variable, while using `==` instead of `===`

PHP wrappers and filters:

* [php wrappers](https://secure.php.net/manual/en/wrappers.php)
* [php filters (base64)](https://secure.php.net/manual/en/filters.convert.php)

<br>

---

## Hardware

Ports with DMA (Direct Memory Access): FireWire, ExpressCard, Thunderbolt, PCI, PCI Express, ...

TMP - Trusted Platform Module

<br>

---

## Just phrases

Static analysis:

* AST - abstract syntax tree
* data flow graph
* control flow graph
* abstract interpretation (set of multiple program states (variable values))
<br>
* taint analysis

Dynamic analysis:

* data to be analysed:

    * program variables
    * syscalls
    * API calls
    * environment change
    <br>
    * instrumentation


<!--MS Office execution contexts:

* Plugins
* OLE - Object Linking and Embedding-->


</article>
