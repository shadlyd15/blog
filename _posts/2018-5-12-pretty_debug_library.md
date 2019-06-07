---
layout: post
title: "Pretty Debug"
subtitle: 'A minimal debug library written in C'
date:       2018-05-12
author: "Shadly"
category: Library
categories: ['Library', 'C', 'C++']
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
  - Debug
  - Library
  - Embedded Systems
  - C
  - C++
  - Arduino
---

# What is PrettyDebug?
A lightweight debug library written in **C/C++**. 
It is designed especially for **embedded systems** keeping in mind the limitation and capabilities of the platform.

# Features :
  - Low memory usage
  - Supports a wide range of compilers and processor architectures
  - Arduino compatible
  - ESP32 & ESP8266 supported
  - Color debug output
  - Very easy to use
  - Supports native C/C++ programs
  
# Prerequisite : 
  - To see debug text in color, ANSI escape sequence supported terminal is needed.
  
# Limitaions : 
  - Arduino serial console does not support ANSI escape sequence. So color output cant not be achived in this terminal. Other expernal terminal e.g. PuTTY will do the work.
  
# API Reference :
 - **Macros :**
```cpp
#define ENABLE_BELL
```

<!-- #define ENABLE_VERBOSE
#define SUPPORT_COLOR_TEXT
#define ENABLE_GLOBAL_DEBUG -->
  