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

We will use [CommandLineUtils](https://github.com/natemcmaster/CommandLineUtils) to parse command line arguments. It is complicated and error-prone for us to use raw `string[] args` in `main function` to parse subcommands and options, so we choose a proper library to do dirty work for us.

[CommandLineUtils](https://github.com/natemcmaster/CommandLineUtils) forks from a library which is ASP.NET team developed for building dotnet cli applications. But years later the team thought it is out of their scope to develop such a library, so they [archived the library](https://github.com/aspnet/Extensions/issues/257#issuecomment-322623120). Fortunately, a developer from ASP.NET team has forked this repository in a private capacity, so we still have an actively developed and well-maintained library.

#### Packaging

The .Net Core's command tool `dotnet` provide two subcommands for packaging: `build` and `pack`, they are used in build normal command line tool and build NuGet package(We can use NuGet package for the delivery of .Net Core Global Tool).

No matter which subcommands we use, we will get a bunch of .exe, .dll, and configuration files. We will use a tool named [wrap](https://github.com/dgiagio/warp), written in Rust, to build a final single .exe file from a bunch of files. It is more convenient for the user to deploy and use only one single file rather than one folder with various files.

**つづく** 
