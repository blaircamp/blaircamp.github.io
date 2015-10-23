---
published: true
title: How to test for file locks on Windows in golang
layout: post
---
{% highlight go %}
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
{% endhighlight %}