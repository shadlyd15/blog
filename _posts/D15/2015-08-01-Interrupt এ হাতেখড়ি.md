---
layout: post
title: "ট্রানজিস্টরের বিকল্প Darlington Driver IC ( ULN2003/ ULN2803 ) ইন্টারফেসিং"
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


যখন আউটপুটের সংখ্যা ২ বা তার বেশি দরকার হবে তখন বারবার ট্রানজিস্টর ব্যবহার না করে ট্রানজিস্টরের বিকল্প Darlington Driver IC ( ULN2003/ ULN2803 ) ব্যবহার করা সহজ। ধরি, আমি দুটি ১২ ভোল্টের মোটর মাইক্রোকন্ট্রোলার দিয়ে চালাতে চাই। তাহলে আমাদের নিচের মত করে সংযোগ দিলেই হবে।

[![ULN2003 Interfacing](https://web.archive.org/web/20150404182419im_/http://d15tech.com/wp-content/uploads/2015/01/uln2003.png)](https://web.archive.org/web/20150404182419/http://d15tech.com/wp-content/uploads/2015/01/uln2003.png)

ULN2003 Interfacing

অপেক্ষাকৃত বেশি কারেন্ট প্রবাহ দরকার হলে একে নিচের মত করে সংযোগ দেয়া যেতে পারে। Darlington Driver IC ( ULN2003/ ULN2803 ) এর ভেতরেই Back EMF প্রোটেকশনের জন্য ডায়োড দেয়া আছে। তাই এতে আলাদাভাবে বাইরে ডায়োড লাগাতে হয় না। ULN2003 এবং ULN2803 এর গঠনগত পার্থক্য হল ULN2003 এ ৭টি এবং ULN2803 এ ৮টি ইনপুট নেয়া যায়।

[![ULN2803 Interfacing](https://web.archive.org/web/20150404182419im_/http://d15tech.com/wp-content/uploads/2015/01/uln2803.png)](https://web.archive.org/web/20150404182419/http://d15tech.com/wp-content/uploads/2015/01/uln2803.png)

ULN2803 Interfacing

তাছাড়া এদের বাকিসব কার্যাবলী একই রকমের। এই IC দিয়ে মোটর ছাড়া আরও অন্যান্য ভারী জিনিস যেমন রিলে কিংবা সলিনয়েড সুইচ ও ইন্টারফেস করা সম্ভব।