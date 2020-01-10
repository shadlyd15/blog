---
layout: post
title: "প্রজেক্টকে একাধিক ফাইল ভাগ করা এবং External Library যুক্ত করা"
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



অনেক সময় দেখা যায় যে একটি সম্পূর্ণ প্রোজেক্ট একটি ফাইলে নিয়ে কাজ করলে কোড অনেক বড় হয়ে যায়। ফলে কোড Maintenance এ দেখা দেয় নানান সমস্যা। এবার অনেক সময় দেখা যায় যে কোন আলাদা লাইব্রেরি যুক্ত করতে হয় অন্যকোন হার্ডওয়্যার ইন্টারফেস করার জন্য। তাই এবারের পরিবেশনা কিভাবে একটি প্রজেক্টকে একাধিক ফাইল ভাগ করা যায় এবং কিভাবে কোডের সাথে লাইব্রেরি যুক্ত করতে হয়। সাধারণত বিভিন্ন User Defined Function কে আলাদা আলাদা ফাইলে রাখা হয়ে থাকে এবং পরে ফাইলটাকে লিঙ্ক করে দেয়া হয়। User Defined Function হল main ফাংশনের বাইরে আলাদা করে নিজের মতো করে তৈরিকৃত ফাংশন। এই ফাংশনকে কাজের সময় নিজের ইচ্ছামত বারবার ব্যবহার করা যায়। সম্পূর্ণ জিনিসটাকে বারবার আর লেখার ঝামেলা পোহাতে হয় না। এখন একটা কোড দেখি।

```cpp
#include <avr/io.h>
#include <util/delay.h>

void  main(){
	int  i=0;
	DDRB=  0xFF;
	PORTB=  0;
	while(1){
		for(i=0;i<8;i++){
			PORTB  =  1<<i;
			_delay_ms(100);
		}
		for(i=7;i>0;i--){
			PORTB  =  1<<i;
			_delay_ms(100);
		}
	}
}
```
এই কোডে যে কাজ হবে তা হল পোর্ট বি এর পিনগুলো একটার পর একটা High হবে এবং স্ক্রলের মত করে উঠানামা করবে। কিন্তু এই স্ক্রলটা যদি আমরা কয়েক জায়গায় ব্যবহার করতে চাই তাহলে কি আমরা বারবার একই for loop লিখেবো? না লিখলেও চলবে। এই জিনিসটাকে আমরা শুধুমাত্র একবারই লিখবো কিন্তু দুটি আলদা ফাংশনে। যাতে প্রয়োজনে এবার এটাকে ব্যবহার করা যায়। তাহলে কোডটা হবে এরকম। আমি এখানে ফাংশন দুটির নাম দিচ্ছি scroll_up এবং scroll_down। এবার কোডটার চেহারা হবে এরকম।

```cpp
#include <avr/io.h>
#include <util/delay.h>

void  scroll_up(){
	int  i=0;
	for(i=0;i<8;i++){
		PORTB  =  1<<i;
		_delay_ms(100);
	}
}

void  scroll_down(){
	int  i=0;
	for(i=7;i>0;i--){
		PORTB  =  1<<i;
		_delay_ms(100);
	}
}

void  main(){
	DDRB  =  0xFF;
	PORTB  =  0;
	while(1){
		scroll_up();
		scroll_down();
	}
}
```

এখানে আমরা দেখলাম scroll_up এবং scroll_down নামের User Definedফাংশন দুটিকে আমরা কিভাবে ব্যবহার করতে পারি।

এখন কথা হচ্ছে আলাদাভাবে ফাংশন লেখার কারণে আমাদের প্রোগ্রামটা বেশ বড়ো দেখাচ্ছে। তাই কোডটাকে একটু সাইজ করার জন্য আমরা এখন এদের বাইরে বের করে আলাদা ফাইলে রাখবো।

প্রথমে Programmers’ Notepad এ গিয়ে দুইটি খালি ফাইল তৈরি করি। আমাদের প্রোজেক্টের main.c টা যে ফোল্ডারে আছে সেই ফোল্ডারে ফাইল দুটি সেভ করি। একটার নাম দেই scroll.h এবং আরেকটার নাম দেই scroll.c। প্রথমে scroll.c ফাইলটা ওপেন করে সেখানে আমরা প্রথমেই scroll.h ফাইলটা কে include করে দেই। এখানে অন্যান্য হেডার ফাইলের মতো করে #include লিখলে চলবে না। #include “scroll.h” লিখতে হবে। ” ” দ্বারা হেডার ফাইল Include করলে কম্পাইলার বুঝে নেয় যে ফাইলটা প্রোজেক্টের Directory তেই আছে। এখন scroll.c তে নিচের কোডটা লিখি।

```cpp
#include "scroll.h"

void  scroll_up(){
	int  i=0;
	for(i=0;i<8;i++){
		PORTB  =  1<<i;
		_delay_ms(100);
	}
}

void  scroll_down(){
	int  i=0;
	for(i=7;i>0;i--){
		PORTB  =  1<<i;
		_delay_ms(100);
	}
}
```
এবার scroll.h এর পালা। scroll.h ফাইলটি ওপেন করে এবার নিচের কোডটুকু লিখি।

```cpp
#include <avr/io.h>
#include <util/delay.h>

void  scroll_up();
void  scroll_down();
```

যেহেতু scroll_up() এবং scroll_down() দুটি ফাংশনেই io.h এবং delay.h এর দরকার আছে তাই এদের এখানে যুক্ত করা হল এবং ফাংশন দুটির নাম লেখা হল।

এবার মূল ফাইলে ( ধরি, main.c ) গিয়ে #include “scroll.h” লিখে দিয়ে আমাদের নিজস্ব হেডার ফাইলটি যুক্ত করে দেই।  
কিন্তু এখানে কম্পাইলার তো আর এতো কিছু বোঝেনা। তাকে scroll.h এ খুঁজতে গিয়ে দেখবে scroll_up() এবং scroll_down() ফাংশন দুটির বর্ণনা যা আছে scroll.c এ। কিন্তু সে তো আর জানে যে এটাকে কোথায় পাবে। তাই তাকে এটা জানাতে হবে। প্রথমে আমরা আমদের makefile টা ওপেন করি। এটাতে নিচের মত একটা লাইন পাওয়া যাবে।

```

# List C source files here. (C dependencies are automatically generated.)

SRC  =  $(TARGET).c
```
এখানে শুধু একটা স্পেস দিয়ে scroll.c ফাইল টা যোগ করে দিতে হবে। নতুন লাইনটা হবে এরকম।
```
SRC  =  $(TARGET).c  scroll.c
```
ব্যাস আমাদের কাজ শেষ। এখন make all দিলেই তৈরি হয়ে যাবে আমাদের কাঙ্ক্ষিত Hex File টি। এখন দেখা যাচ্ছে আমাদের মূল কোডটাকে একেবারে ফ্রেশ।

ঠিক একইভাবে আমরা যদি কোন External Library যুক্ত করতে চাই তাহলেও একইভাবে শুধু Source File টা যোগ করে দিলেই কাজ হয়ে যাবে। ঠিক যেমন LCD এর লাইব্রেরিটি। এখানে lcd.c ফাইলটাকে যোগ করলেই কাজ হয়ে যাবে।