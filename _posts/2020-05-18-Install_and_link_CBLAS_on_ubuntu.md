---
layout: post
title: "Compile and link CBLAS on Ubuntu"
subtitle: 'A guide to compile and use CBLAS in cpp projects'
date:       2020-05-18
author: "Shadly"
category: Mathematics
categories: ['Tutorial', 'Mathematics', 'Lineara Algebra', 'BLAS']
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
  - Mathematics
  - Tutorial
  - Lineara Algebra
  - CBLAS
  - BLAS
---

## What is BLAS
The BLAS (Basic Linear Algebra Subprograms) are routines that provide standard building blocks for performing basic vector and matrix operations. The Level 1 BLAS perform scalar, vector and vector-vector operations, the Level 2 BLAS perform matrix-vector operations, and the Level 3 BLAS perform matrix-matrix operations. Because the BLAS are efficient, portable, and widely available, they are commonly used in the development of high quality linear algebra software, LAPACK for example. -- <cite>[http://www.netlib.org/blas](http://www.netlib.org/blas/#_presentation)</cite>

## What is CBLAS 
CBLAS is the C interface for legacy BLAS. It is a wrapper around the highly optimized linear algebra library libblas. If you are into high performance computing, then it is a thing for you. To be able to use this interface, you need libblas installed in your system. 

## Install libblas-dev
To install libblas-dev in your system run the following commands
``` bash
sudo apt-get update
sudo apt-get install libblas-dev
```

## Locate libblas location
Run the following command to locate libblas.a. 
```bash
whereis libblas
```
For my case it was "/usr/lib/x86_64-linux-gnu/libblas.a". We will need this location later to compile CBLAS.


## Download and extract CBLAS
Download CBLAS from [this](http://www.netlib.org/blas/blast-forum/cblas.tgz) link.
Extract the tgz file with the following command. 
```bash
tar xzvf blas.tgz
```

## Compile CBLAS
Go to the extracted folder and open the file "Makefile.in". Find the line "BLLIB = ". Change the location of the libblas.a with your location. In my case the line looked like below. 
```bash
BLLIB = /usr/lib/x86_64-linux-gnu/libblas.a
```
Save the file and run make command.
```bash
make
```
If everything goes right, CBLAS will be compiled in a few seconds. You will find the archived library in lib folder with the name "cblas_LINUX.a"

## Use static CBLAS library in cpp project
To use cblas_LINUX.a in your cpp project, just copy the library in your root folder and rename it "libcblas.a". Also copy the headers cblas.h and cblas_f77.h in that folder. Here is a simple example in cpp which calculates the Euclidean norm of a sample vector. Save the code with filename "norm.cpp"

```cpp
#include <iostream>
#include <math.h>

extern "C" {
  #include "cblas.h"
}

using namespace std;

int main(int argc, char const *argv[]){
	int n;
	double euclidean_norm = 0;

	cout << "Enter the size of the vector : ";
	cin >> n;

	// Creating sample vector
	double * V = new double[n];
	if(V){
		for (int i = 0; i < n; ++i){
			V[i] = (double)1 / (i + 1);		
		}
	}
	
	// Calculating the euclidean norm with cblas function "cblas_dnrm2"
	euclidean_norm = cblas_dnrm2(n, V, 1);
	cout << "Euclidean Norm with cblas : " << euclidean_norm << endl;

	delete V;
	return 0;
}

```

Run the following command to compile your code
```bash
g++ norm.cpp -L. -lcblas -lblas
```

If everything goes alright, you will have your compiled binary without any error.