---
layout: post
title: "মাইক্রোকন্ট্রোলারের সাথে Alphanumeric LCD ইন্টারফেস"
subtitle: 'Bangla tutorial series on Microcontroller, retreived from my previous blog d15tech.com. Dated here according to the original published date.'
date:       2014-01-07
author: "Shadly"
category: Microcontroller
categories: ['Tutorial']
header-img: "img/post-bg-js-module.jpg"
<!-- catalog: true -->
tags:
  - Microcontroller
  - মাইক্রোকন্ট্রোলার
  - Atmega8
  - Bangla
  - AVR
  - বাংলা
---


এতক্ষণ আমরা আমদের আউটপুট শুধু LED তেই সীমাবদ্ধ রেখেছি। কিন্তু আর না। এবার আমরা আমাদের আউটপুটকে সরাসরি লেখা আকারে দেখব। এর জন্য যে ডিভাইসটি দরকার হবে তা Alphanumeric LCD। যদিও এর নাম Alphanumeric LCD তা সত্ত্বেও এই ডিসপ্লেতে Alphabetএবং Number ছাড়াও অন্যান্য সিম্বলও দেখানো সম্ভব। ইচ্ছা করলে Custom Character ও বানিয়ে নেয়া সম্ভব। খুবই কাজের জিনিস এই Alphanumeric LCD। বাজারে প্রচলিত Alphanumeric LCD গুলোর মধ্যে সবচেয়ে বেশি ব্যবহৃত হয় 16×2 এলসিডি ডিসপ্লে। 16×2 অর্থ হচ্ছে এতে দুইটি লাইন আছে এবং প্প্রত্যেক লাইনে ১৬টি করে character রয়েছে। এছাড়াও বাজারে 24×4, 24×2, 8×2,16×1 ইত্যাদি ধরণের LCD পাওয়া যায়। তবে প্রায় সবগুলো LCD তেই পিন সংখ্যা 16। এর ১৬ টি পিনের মধ্যে ৮ টি Data Line পিন, ৪ টি Control Line পিন, ২ টি Power Line পিন, ২ টি Backlit LED পিন এবং বাকি ১টি হল Contrast পিন। নিচে Alphanumeric LCD এর পিন বিন্যাস ও পিনের কাজগুলো দেয়া হল।

[![LCD Pinout](https://web.archive.org/web/20150405043123im_/http://d15tech.com/wp-content/uploads/2015/01/lcd.jpg)](https://web.archive.org/web/20150405043123/http://d15tech.com/wp-content/uploads/2015/01/lcd.jpg)

LCD Pinout

**কনট্রাস্ট পিন**  
LCD এর তিন পিনটি হল কনট্রাস্ট পিন। এই পিনে ০ থেকে ৫ ভোল্টের এর মধ্যে ভোল্টেজ পরিবর্তন করে LCD এর কনট্রাস্ট পরিবর্তন করা যায়।

**রেজিস্টার সিলেক্ট পিন**  
LCD তেও মাইক্রোকন্ট্রোলারের মত Register আছে। রেজিস্টার সিলেক্ট পিনটি High এবং Low করে Register Select করতে হয়। এই পিনটি যখন Low থাকবে তখন LCDটি command মোডে থাকবে অর্থাৎ এটি বিভিন্ন কমান্ড নেয়ার জন্য প্রস্তুত হবে। আর এই পিনটিকে যদি high থাকে LCD টি Character Display Mode থাকবে।

**Read/Write পিন**  
LCD টির 5 নাম্বার পিনটি হচ্ছে Read/Write পিন। এই পিনটি যখন Low থাকবে তখন LCDটি command বা Character Write করার জন্য প্রস্তুত হবে। আর এই পিনটিকে যদি high থাকে LCD টি থেকে read করা যাবে।

**Enable পিন**  
এই পিনটি ডাটা লাইন Write মোডে থাকবে না Read মোডে থাকবে তা নির্ধারণ করে।এই পিনটি যখন Low থাকবে তখন LCD এর ডাটা লাইনগুলো ডাটা Write করার জন্য প্রস্তুত হবে। আর এই পিনটিকে যদি high থাকে LCD টির ডাটা লাইন থেকে ডাটা read করা যাবে।

**ডাটা লাইন**  
D0-D7 এই আটটি পিন হচ্ছে LCD এর ডাটা লাইন। এই ডাটা লাইনের ৪ টি কিংবা ৮ টি ব্যবহার করে LCDতে Character Displayকরানো যায়। যখন ৪ টি ডাটা লাইনে ডাটা পাঠানো হয় ক্ষেত্রে D4 থেকে D7 এই চারটি পিন ব্যবহার করা হয়। বিভিন্ন Character কে ডিসপ্লে করার জন্য ডাটা লাইনের বিন্যাস নিচে দেয়া হল।

[![Character Chart](https://web.archive.org/web/20150405043123im_/http://d15tech.com/wp-content/uploads/2015/01/Chacter-Chart.png)](https://web.archive.org/web/20150405043123/http://d15tech.com/wp-content/uploads/2015/01/Chacter-Chart.png)

Character Chart

তবে এতো সবকিছু Manually না করে সরাসরি LCD এর লাইব্রেরি ব্যবহার করা যায়। ইন্টারনেটে অসংখ্য LCD Library আছে। এর মধ্যে  [Extreme Electronics এর লাইব্রেরিটি](https://web.archive.org/web/20150405043123/http://extremeelectronics.co.in/avrtutorials/code/lcdlibv20.zip)  বিশেষভাবে উল্লেখ্য। তুমি চাইলে নিজেও নিজের লাইব্রেরি তৈরি করে নিতে পারো। তবে এতো ঝামেলায় না গিয়ে এখানে আমি Extreme Electronics এর লাইব্রেরিটির বিভিন্ন ফাংশন সম্পর্কে আলোচনা করছি।

**LCDInit(LS_BLINK|LS_ULINE)**  
এখানে আর্গুমেন্ট LS_BLINK ব্যবহার করলে পাওয়া যাবে blinking cursor. LS_ULINE ব্যবহার করলে পাওয়া যাবে underlined cursor।

**LCDWriteString(“Welcome”)**  
এই ফাংশন ব্যবহার করে LCD তে কোন String প্রদর্শন করা যাবে।

**LCDWriteInt(int val,unsigned int field length)**  
এই ফাংশন ব্যবহার করে ডিসপ্লেতে Integer দেখানো সম্ভব। এই ফাংশন ব্যবহার করলে ডিসপ্লেতে val এর মান প্রদর্শন করবে field length সংখ্যক ঘর পর্যন্ত।

**LCDGotoXY(uint8_t x,uint8_t y)**  
ডিসপ্লের যেকোনো জায়গায় জাম্প করার জন্য এই ফাংশনটি ব্যবহার করা হয়।

**LCDClear()**  
ডিসপ্লের সব লেখা মুছে ফেলতে এই ফাংশন ব্যবহার করা হয়।

**LCDWriteStringXY(x,y,msg)**  
ডিসপ্লের কোন নির্দিষ্ট স্থানে স্ট্রিং প্রদর্শন করার জন্য এই ফাংশন ।

**LCDWriteIntXY(x,y,num,field_length)**  
ডিসপ্লের কোন নির্দিষ্ট স্থানে Integer প্রদর্শন করার জন্য এই ফাংশন ।

শুধু ফাংশন জানলেই হবে না কিভাবে এই লাইব্রেরিটাকে আমাদের LCD এর জন্য কনফিগার করবো তাও জানতে হবে। এবার তাহলে তাই দেখা যাক। lcd.h fileটি খুললে আমরা এরকম একটা জিনিস দেখতে পাবো। এখানে সব বিস্তারিত বলে দেয়াই আছে কোন পিন কোথায় কানেক্ট করতে হবে।
```cpp
/**********************************************************************************************/
//LCD CONNECTIONS
/**********************************************************************************************/

#define LCD_DATA B //Port PB2-PB5 are connected to D4-D7
#define LCD_DATA_POS 2
#define LCD_E D // যে পোর্টে Enable পিনটি থাকবে সেই পোর্টের নাম। এখানে D
#define LCD_E_POS PD5 // যে পিনে Enable পিনটি থাকবে সেই পিনের নাম। এখানে PD5
#define LCD_RS D // যে পোর্টে RS পিনটি থাকবে সেই পোর্টের নাম। এখানে D
#define LCD_RS_POS PD7 // যে পিনে RS পিনটি থাকবে সেই পিনের নাম। এখানে PD7
#define LCD_RW D // যে পোর্টে RW পিনটি থাকবে সেই পোর্টের নাম। এখানে D
#define LCD_RW_POS PD6 // যে পিনে RW পিনটি থাকবে সেই পিনের নাম। এখানে PD6
/************************************************************************************************
```
আর এরকম জায়গায় শুধু তোমার LCD এর ধরণের লাইনটা আনকমেন্ট করে দিলেই হবে।

```cpp
//#define LCD_TYPE_202 //For 20 Chars by 2 lines
//#define LCD_TYPE_204 //For 20 Chars by 4 lines
//#define LCD_TYPE_162 //For 16 Chars by 2 lines
//#define LCD_TYPE_164 //For 16 Chars by 4 lines
```
