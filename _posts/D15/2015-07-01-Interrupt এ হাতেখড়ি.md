---
layout: post
title: "Interrupt এ হাতেখড়ি"
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


Interrupt এর অর্থ হল মাঝপথে বাঁধা দেয়া। মাইক্রোকন্ট্রোলার এ কোন কোড চলার সময় যদি আমরা ঐ কোডের কাজ বাদ দিয়ে অন্যকোন কাজ করতে চাই তাহলে Interrupt ব্যবহার করতে হবে। ধরি, while loop এর ভেতরে একটি LED Blink করার জন্য কোড করা আছে এভাবে –
```cpp
#include <avr/io.h> // io.h Header File এর সংযুক্তি
#include <util/delay.h> // delay.h Header File এর সংযুক্তি
int  main(void) { // main ফাংশনের শুরু{
	DDRC  |=  1  <<  5;  // পোর্ট সি এর ৫ নম্বর পিনকে আউটপুট হিসেবে নেয়া হল
	while  (1){  // Infinite Loop
		PORTC|=(1<<5);  // পিন নম্বর ৫ কে High করা হল
		_delay_ms(200);  //কোডেটাকে কিছু সময়ের জন্য থামানো হল
		PORTC&=~(1<<5);  // পিন নম্বর ৫ কে Low করা হল
		_delay_ms(200);  //কোডেটাকে আবার কিছু সময়ের জন্য থামানো হল
	}  // Infinite Loop শেষ
}
```
যেহেতু C Program লাইন বাই লাইনে Execute হয় তাই এখানেও একই জিনিস হবে। এখন যদি ৪ নম্বর লাইন Execute হবার সময় মাইক্রোকন্ট্রোলার এ Interrupt Call করা হয় তাহলে সে ৫ নম্বর লাইনে না গিয়ে সরাসরি Interrupt এর জন্য নির্ধারিত কাজে চলে যাবে। এই নির্ধারিত কাজ শেষ হলে সে আবার ৫ নম্বর লাইন থেকে কাজ শুরু করবে। Interrupt এর জন্য নির্ধারিত কাজ main() ফাংশনের বাইরে থাকে এবং Interrupt এর ধরণ অনুযায়ী বিভিন্নভাবে execute হয়ে থাকে। মাইক্রোকন্ট্রোলার এর বিভিন্ন ধরণের Interrupt এর মধ্যে ADC Conversion Complete , Watchdog Timeout Interrupt, UART Rx Complete, 2-wire Serial Interface, Timer/Counter2 Compare Match , Timer/Counter Overflow, External Interrupt অন্যতম। এদের মধ্যে একমাত্র Hardware Interrupt হল External Interrupt। এই ধরণের Interrupt ব্যবহার করে বাটন চাপার মাধ্যমে কিংবা পিন লো করে মাইক্রোকন্ট্রোলার এ Interrupt এর নির্দেশ পাঠানো যায়। Interrupt এর জন্য নির্ধারিত ফাংশন হল ISR (Interrupt Service Routine)। এই ফাংশনের আর্গুমেন্টে বিভিন্ন ধরণের Interrupt এর জন্য নির্ধারিত ভেক্টর দিয়ে কাজ করতে হয়। মাইক্রোকন্ট্রোলার এর বিভিন্ন ধরণের Interrupt এর জন্য বিভিন্ন ধরণের ভেক্টর AVR-LibC তে নির্ধারণ করা আছে।

আমরা এখন দেখব কিভাবে বাটন চেপে মাইক্রোকন্ট্রোলার এ Interrupt নির্দেশ পাঠাতে হয়।

Interrupt নিয়ে কাজ করতে হলে আমদের প্রথমেই কিছু জিনিস ঠিকঠাক করে দিতে হবে। Interrupt সম্পর্কিত সকল ফাংশন Interrupt.h এ আছে। তাই Interrupt ব্যবহার করতে হলে অবশ্যই এই হেডার ফাইলটি যুক্ত করতে হবে। এখন আমাদের Global Interrupt Enable করতে হবে। এটি করতে হলে আমাদের Status Register (SREG) এর ৭ নম্বর বিটটি হাই করে দিতে হবে। এখন আমাদের MCU Control Register (MCUCR) এর Interrupt Sense Control বিট গুলোকে Configure করে আমদের চাহিদামত Interrupt সেট করে নিবো। Rising Edge বলতে বোঝায় লো থেকে হাই অবস্থায় যাওয়ার মধ্যবর্তী অবস্থাকে ঠিক একইভাবে Falling Edge বলতে বোঝায় হাই থেকে লো অবস্থায় যাওয়ার মধ্যবর্তী অবস্থাকে।

[![in0_table - D15 Technologies](https://web.archive.org/web/20150404203855im_/http://d15tech.com/wp-content/uploads/2015/01/in0_table.png)](https://web.archive.org/web/20150404203855/http://d15tech.com/wp-content/uploads/2015/01/in0_table.png)

[![in1_table - D15 Technologies](https://web.archive.org/web/20150404203855im_/http://d15tech.com/wp-content/uploads/2015/01/in1_table.png)](https://web.archive.org/web/20150404203855/http://d15tech.com/wp-content/uploads/2015/01/in1_table.png)

এখন আমাদের কাজ হল মাইক্রোকন্ট্রোলার এর Interrupt এর পিনকে চালু করা। এটা করার জন্য আমাদের General Interrupt Control Register (GICR) এর কিছু পিনকে হাই করতে হবে। আমরা মাইক্রোকন্ট্রোলার এর যে পিনে Interrupt ইনপুট দিতে চাই সেই বিটকে হাই করে দিবো। INT0 কে চালু করতে চাইলে INT0, INT1 কে চালু করতে চাইলে INT1 এবং INT2 কে চালু করতে চাইলে INT2 বিটকে হাই করে দিতে হবে।[![gicr - D15 Technologies](https://web.archive.org/web/20150404203855im_/http://d15tech.com/wp-content/uploads/2015/01/gicr.png)](https://web.archive.org/web/20150404203855/http://d15tech.com/wp-content/uploads/2015/01/gicr.png)

এবার সময় হল Global Interrupt Enable করার। এটা করার জন্য আমাদের যা করতে হবে তা হল শুধুমাত্র sei() এই ফাংশনটা কল করা। এই ফাংশনটা Interrupt.h হেডার ফাইলে আছে। এই ফাংশনটা কল করলে Interrupt ফাংশন চালু হয়ে যাবে এবং সে Interrupt ইনপুট নিতে পারবে। অন্যথায় পারবে না। ঠিক একইভাবে Global Interrupt Disable করার জন্য আমাদের cli() এই ফাংশনটা ব্যবহার করতে হবে।

এবার একটা কোড দেখা যাক।

```cpp
#include <avr/io.h>
#include <avr/interrupt.h>
#include <util/delay.h>

void  main(){
	DDRB  =  0x00;
	PORTB  =  0x00;
	MCUCR|=(1<<ISC01);  //INT0 কে Falling Edge এ কনফিগার করা হল।
	GICR|=(1<<INT0);  //INT0 কে Interrupt হিসাবে Initialize করা হল।
	sei();  // Global Interrupt Enable করা হল।
	while(1){;}
}

ISR(INT0_vect){
	PORTB  ^=  0xFF;
}
```
ব্যাস হয়ে গেল বেসিক External Interrupt এর কাজ। এই কোড মাইক্রোকন্ট্রোলার এ দিলে while লুপের ভিতরে সারাজীবন থেকে পোর্ট বি এর সব পিন লো হয়ে থাকার কথা। কিন্তু INT0 এর যখন Falling Edge হবে তখন কোড while লুপ থেকে বের হয়ে সরাসরি ISR(INT0_vect) ফাংশনে চলে যাবে এবং মাইক্রোকন্ট্রোলার এর পোর্ট বি কে toggle করে দিবে। এখানে INT0_vect হল INT0 এর জন্য নির্ধারিত ভেক্টর।

[![interrput  - D15 Technologies](https://web.archive.org/web/20150404203855im_/http://d15tech.com/wp-content/uploads/2015/01/interrput.jpg)](https://web.archive.org/web/20150404203855/http://d15tech.com/wp-content/uploads/2015/01/interrput.jpg)

চিত্রের মত সংযোগ দিলে সবকিছু ঠিক থাকলে বাটন চাপার সাথে সাথে মাইক্রোকন্ট্রোলার এর পোর্ট বি কে toggle হয়ে যাবে।