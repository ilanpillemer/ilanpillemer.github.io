---
layout: post
title: "apue, example 1"
---
The first example in [APUE](http://www.apuebook.com "Advanced Programming in the Unix Environment") shows the use of the C standard library to
implement a very simple `ls`. The C standard library abstracts away a
number of issues, namely :
* the system calls themselves are unbuffered, so the management of
  providing a buffer and then parsing the directory entries from that
  buffer is a complication abstracted by the `C` standard library.
* different operating systems implement the `C` struct representing a
  directory entry.
* the standard library in `C` abstracts this and optimises the manner
  in which directory entries are parsed for each operating system;
  similarly the Go authors have written a standard library for Go. The
  standard library in Go; although it has to use the `C` system calls;
  does not use the `C` standard library.

## Gotchas

* There must be no empty lines between the `C` pre-amble and the `import "C"`
* when converting a `C` string to a `Go` string, you pass the address of the first element in the null terminated char array representing the string, such as `&dirent.d_name[0]` to the `C.GoString(__)` function.

## Code Examples 

As a result there are two examples in Go for this example:


* An example using `CGo` to call the `C` standard library to implement
  a very simple `ls`.
  
```
package main

/*
#include <dirent.h>
#include <stdlib.h>
*/
import "C"

import (
	"fmt"
	"os"
	"unsafe"
)

// dirent.h provides opendir, readdir, closedir
// stdlib.h provides free

// these calls are not sys calls, but standard library calls
// directory entries are represented different in different
// unix systems, and the system calls vary.

func main() {
	if len(os.Args) != 2 {
		fmt.Println("usage: myls directory_name")
		os.Exit(1)
	}
	arg := C.CString(os.Args[1])
	defer C.free(unsafe.Pointer(arg))

	dir, err := C.opendir(arg)
	if err != nil {
		fmt.Printf("can't open %s: %v\n", os.Args[1], err)
		os.Exit(1)
	}
	defer C.closedir(dir)

	for dirent := C.readdir(dir); dirent != nil; dirent = C.readdir(dir) {
		name := C.GoString(&dirent.d_name[0])
		fmt.Println(name)
	}
}
```

* An example using `Go` to call the `Go` standard library to implement
  a very simple `ls`.
  
```
package main

import (
	"fmt"
	"io/ioutil"
	"os"
)

// dirent.h provides opendir, readdir, closedir
// stdlib.h provides free

// the equivalent standard library calls in
// golang are provide by io.ioutil

// the golang authors decided not to
// return the "." and ".." directory entries

func main() {
	if len(os.Args) != 2 {
		fmt.Println("usage: myls directory_name")
		os.Exit(1)
	}

	dir, err := ioutil.ReadDir(os.Args[1])
	if err != nil {
		fmt.Printf("can't open %s: %v\n", os.Args[1], err)
		os.Exit(1)
	}

	fmt.Println(".")
	fmt.Println("..")
	for _, f := range dir {
		name := f.Name()
		fmt.Println(name)
	}
}
```


## Functional Differences

* The C standard library call's directory entries are not returned in a determined manner. They are not ordered and may be returned in any order. The underlying unbuffered system call also does not return the entries in a determined or sorted order.

* The Go standard library returns the directory entries in a sorted lexical order. As the underlying system call does not sort the entries, this sorting is done within the standard library after calling the underlying unbuffered system call.

* The C standard library returns all directory entries. This includes the directory entries for `.` and `..`. The underlying system call also returns these entries.

* The Go standard library does not return `.` and `..` in the list of directory entries. This means that the Go standard library removes these entries after calling the underlying system call, as the system call does return these entries.
