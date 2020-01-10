---
layout: post
title: "Pretty Debug"
subtitle: 'A minimal debug library written in C'
date:       2018-05-12
author: "Shadly"
category: Library
categories: ['Debug','Library', 'C', 'C++']
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

# PrettyDebug
A lightweight debug library written in **C/C++**. 
It is designed especially for **embedded systems** keeping in mind the limitation and capabilities of the platform.
  - Low memory usage
  - Supports a wide range of compilers and processor architectures
  - Arduino compatible
  - ESP32 & ESP8266 supported
  - Color debug output
  - Very easy to use
  
## How to use

- **Attach Debug Stream :**
```cpp
  ATTACH_DEBUG_STREAM(&Serial);
``` 

- **Print Green OK Message :**
```cpp
  DEBUG_OK("An Example OK Message From %s, "PrettyDebug");
```

- **Print Red ERROR Message :**
```cpp
  DEBUG_ERROR("An Example ERROR Message From %s, "PrettyDebug");
```

- **Print Red ERROR Message :**
```cpp
  DEBUG_ERROR("An Example ERROR Message From %s, "PrettyDebug");
```

- **Print Cyan ALERT Message :**
```cpp
  DEBUG_ALERT("An Example ALERT Message From %s, "PrettyDebug");
```

- **Print Yellow WARNING Message :**
```cpp
  DEBUG_WARNING("An Example WARNING Message From %s, "PrettyDebug");
```

- **Print Variable with Variable Name :**
```cpp
  DEBUG_VARIABLE("%d", Sample_Value);
``` 

- **Print Array with Name :**
```cpp
  DEBUG_ARRAY(Sample_Array, 16, "%02X");
``` 

**Print Current Location in Code :**
```cpp
  DEBUG_TRACE();
``` 

## Arduino Example 

```cpp
  #include "PrettyDebug.h"

  int Sample_Variable = 123;
  int Sample_Array[] = {1, 2, 3, 4, 5};

  void setup(){
      Serial.begin(115200);
      ATTACH_DEBUG_STREAM(&Serial);

      DEBUG_OK("Pretty Debug Example Sketch");
      DEBUG_TRACE();

      DEBUG_OK("An Example OK Message From %s, "PrettyDebug");
      DEBUG_ERROR("An Example ERROR Message From %s, "PrettyDebug");
      DEBUG_ERROR("An Example ERROR Message From %s, "PrettyDebug");
      DEBUG_ALERT("An Example ALERT Message From %s, "PrettyDebug");
      DEBUG_WARNING("An Example WARNING Message From %s, "PrettyDebug");
      DEBUG_VALUE(Sample_Value, "%d");
      DEBUG_ARRAY(Sample_Array, 16, "%02X");
  }

  void loop(){

  }
 ```

## Sample Output

<div style="text-align:center"><img src ="https://raw.githubusercontent.com/shadlyd15/prettydebug/master/images/output.png" alt ="Sample Output"/></div>
  