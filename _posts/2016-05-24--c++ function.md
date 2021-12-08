---
layout: post
title:  "c++ fuction"
date:   2016-05-24 14:20:09 +0800
categories: program
tag: program
---

# std::cerr
std::cerr（console error）是ISO C++标准错误输出流,与std::cout不同，ISO C++要求当cerr被初始化后，cerr.flags() & unitbuf非零（保证流在每次输出操作后被刷新），且cerr.tie()返回&cout。cout对应于标准输出流，默认情况下是显示器。这是一个被缓冲的输出，可以被重定向。cerr对应标准错误流，用于显示错误消息。默认情况下被关联到标准输出流，但它不被缓冲，也就说错误消息可以直接发送到显示器，而无需等到缓冲区或者新的换行符时，才被显示。一般情况下不被重定向。
