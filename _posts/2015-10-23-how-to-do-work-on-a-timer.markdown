---
published: false
title: How to do work on a Timer
layout: post
---
package main

import "fmt"
import "time"

func main() {
      interval := time.Millisecond * 500
	ticker := time.NewTimer(interval)
    go func() {
        for t := range ticker.C {
            fmt.Println("Tick at", t)
	    ticker.Reset(interval)
        }
    }()
    time.Sleep(time.Millisecond * 1600)
    ticker.Stop()
    fmt.Println("Ticker stopped")
    interval = time.Millisecond * 1000
    ticker.Reset(interval )
   time.Sleep(time.Millisecond * 2200)
   fmt.Println("Ticker stopped")
}