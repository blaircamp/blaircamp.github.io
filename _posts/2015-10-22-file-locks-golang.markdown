---
published: true
title: How to test for file locks on Windows in golang
layout: post
tags:
  - golang
categories:
  - golang
---




I needed a way to test if a file was still locked by another process.  I ended up creating the function below.  It uses the windows.Open function that eventually calls the win api function CreateFile.  I try to get the Read/Write, Create and Append permission for the file.  If another process is using that file it will fail and return an error, otherwise it will return a file handle that will be closed using windows.Close.

```golang
import "golang.org/x/sys/windows"
func IsFileLocked(path string) bool {
ret := false
f, err := windows.Open(path, os.O_RDWR|os.O_CREATE|os.O_APPEND, 0666)
if err != nil {
fmt.Println(“File is locked”, err)
ret = true
}
windows.Close(f)
return ret
}
```
