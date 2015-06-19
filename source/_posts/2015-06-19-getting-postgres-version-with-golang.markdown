---
layout: post
title: "Getting Postgres Version with Golang"
date: 2015-06-19 16:49:12 -0500
comments: true
categories:
---
I've been looking at some monitoring solutions for my network at home. I've used [nagios](https://www.nagios.org/) in the past but it has quite a few drawbacks. Most noticably, the whole fact that it doesn't scale to millions of nodes or use a popular javascript framework for its front end pretty much makes it a no-go for my needs. Thanks to my background in technology, I know that the next best option is to just write something myself. As I stated earlier, nagios is a no-go and I need something that is... so I chose [go-lang](https://golang.org/). The fact that go is in the name tells me it will work. Let's get started!
#### Plumbing:
```
brew install go
mkdir ~/go
export GOPATH=~/go
export PATH=$PATH:$GOPATH/bin
go get github.com/lib/pq
mkdir gogios && cd gogios
touch gogios.go
```
#### Now we code:
```
package main
import (
  _ "github.com/lib/pq"
  "database/sql"
  "fmt"
  "log"
)

func main() {
  db, err := sql.Open("postgres", "user=rob dbname=postgres sslmode=disable")
  if err != nil {
    log.Fatal(err)
  }

  var version string
  err = db.QueryRow("select version()").Scan(&version)
  if err != nil {
  	log.Fatal(err)
  }
  fmt.Println(version)
}
```
#### Run it:
```
go run gogios.go
```
#### Output:
```
PostgreSQL 9.4.4 on x86_64-apple-darwin14.3.0, compiled by Apple LLVM version 6.1.0 (clang-602.0.49) (based on LLVM 3.6.0svn), 64-bit
```
Now, while we haven't replaced 100% of nagios, we're off to a great start and probably only have a few hundred hours of coding left. Good thing it is the weekend!
