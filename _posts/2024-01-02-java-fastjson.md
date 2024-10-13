---
layout: post
author: d1sbb
title: "[分享] 全版本fastjson反序列化漏洞分析"
date: 2024-01-02
music-id: 
permalink: /archives/2024-01-02/1
description: "对fastjson反序列化漏洞的学习"
categories: "网络安全"
tags: [java, fastjson]
---

## FastJson漏洞链特点
在反序列化时，parse触发了set方法，parseObject同时触发了set和get方法，由于存在这种`autoType`特性。如果`@type`标识的类中的setter或getter方法存在恶意代码，那么就有可能存在fastjson反序列化漏洞。

FastJson反序列化和原生反序列化利用不同的点：  
1. FastJson不需要实现Serializable
2. 不需要变量不是transient/可控变量：
    1. 变量有对应的setter
    2. 或是public/static
    3. 或满足条件的getter
3. 反序列化入口点不是readObject，而是setter或者是getter
4. 执行点是相同的：反射或者类加载


> [参考文章1](https://klearcc.github.io/post/javasec_fastjson%E5%85%A8%E7%89%88%E6%9C%AC/#%E5%B0%86json%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E4%B8%BA%E7%B1%BB)
> [参考文章2](https://xz.aliyun.com/t/12728?time__1311=GqGxu7G%3DiQdmqGN4CxUxYTSFTG8CdHqaW4D#toc-5)