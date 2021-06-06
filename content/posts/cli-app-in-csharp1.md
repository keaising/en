---
title: "CLI App in C#: 1"
date: 2019-05-11
draft: false
tags: ["cli in csharp", "dotnet core"]
categories: ["programming"]
---

This is the first post of CLI App in C# series. This series aims to illustrate every detail of how to write a command tool in C#, based on .Net Core. I will explain most content from outlining an app, choosing a proper platform, packaging code into a single binary execute file to automating build in CICD.

+ First, functions that will be included in demo App, technologies will be used and why use them.
+ Second, most code in demo App.
+ Last, packaging, how to build a final binary suitable for delivery.

So, let's begin.

## Introduction of demo App

If you use `git` as your daily version control tool, you will be familiar with the pattern of `Command SubCommand Option`, for example `git push origin master` / `git clone https://github.com/torvalds/linux.git`, we will create a demo App follows this pattern.

The primary function of the demo App will be to generate various numbers. It can generate different numbers depends on different subcommands and options. It is named `gen` and all the functions are included below.

| command         | function                                           | result             |
| ----            | ----                                               | ----               |
| `gen mobile`    | generate a random Chinese phone number             | 15810885678        |
| `gen id`        | generate a 18 bit Chinese ID number                | 210765199707122345 |
| `gen id -l 110` | generate a Chinese ID number has a prefix of `110` | 110765199707122345 |
| `gen id -s`     | generate a 15 bit Chinese ID number                | 456719970712234    |
| `gen -v`        | print version info                                 |                    |

## Introduction of Jargon

In the full command of `gen id -l 110`, there are some terms we use in command tool.

| command | Jargon     |
| ----    | ----       |
| `gen`   | Command    |
| `id`    | SubCommand |
| `-l`    | Option     |
| `'110'` | Verbs      |

## Technology Selection

#### Framework

We use dotnet core 2.1，which is the most recent LTS version of the .Net Core platform. It allows us to run our tool in not only Windows, but also macOS and Linux. We can use C# and develop very fast.

#### Parsing command line arguments 

We will use [`CommandLineUtils`](https://github.com/natemcmaster/CommandLineUtils) to parse command line arguments. It is complicated and error-prone for us to use raw `string[] args` in `main function` to parse subcommands and options, so we choose a proper library to do dirty work for us.

[`CommandLineUtils`](https://github.com/natemcmaster/CommandLineUtils)的前身是ASP.NET团队在开发dotnet core的过程中，为开发dotnet cli而开发的扩展库，后来ASP.NET团队觉得这个功能跟ASP.NET的开发关系不大，于是转入[不活跃状态](https://github.com/aspnet/Extensions/issues/257#issuecomment-322623120)，后续被ASP.NET团队的一个开发小哥以[私人身份fork并继续维护](https://github.com/aspnet/Extensions/issues/257#issuecomment-326726754)，于是就有了现在用的这个库。

#### Packaging

dotnet core提供打包工具`build`和`pack`，分别用于打包普通控制台应用和nuget包（用于dotnet core global tool的分发）。

dotnet core的打包工具的打包结果会是一大堆exe、dll和相关配置文件，为了最终获得一个单独的exe文件，使用[wrap](https://github.com/dgiagio/warp)库来将所有的文件打包成一个exe，方便部署和使用。

**つづく** 
