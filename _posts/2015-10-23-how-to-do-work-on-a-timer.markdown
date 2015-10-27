---
published: false
title: How to do work on a timer in a windows service [golang]
layout: post
tags: [golang]
categories: [golang]
---
{% highlight go %}
package main
import "github.com/kardianos/service"
import "time"

type program struct{
exit chan struct{}
}

func longProcessingFunc(){
time.Sleep(60 * time.Second)
}

func (p *program) Start(s service.Service) error{
go p.run()
return nil
}

func(p *program) Stop(s service.Service) error{
 close(p.exit)
}

func (p *program) run() error{
ticker := time.NewTimer( 60 * time.Second)
for{
case <-ticker.C:
ticker.Stop()
//DO SOMETHING
longProcessingFunc()
ticker.Reset(5 * time.Second)
case <-p.exit:
ticker.Stop()
return nil
}
}

func main(){
svcConfig := &service.Config{
Name: "ServiceName"
DisplayName:"ServiceDisplayName"
Description:"ServiceDescription"
prg := &program{}
s,err := service.New(prg,svcConfig)
if err != nil{
panic(err)
}
err = s.Run()
if err != nil{
panic(err)
}

}

{% endhighlight %}