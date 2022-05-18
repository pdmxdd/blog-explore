---
title: "Consume API from Scala"
weight: 100
date: 2022-05-17
---

First problem. I need to make HTTP requests from Scala and unpack the associated CSV records.

Let's check out the `scala.io.Source` package of the `Scala` standard library.

According to the [Scala Docs io.Source](https://www.scala-lang.org/api/current/scala/io/Source$.html) there is a `fromURL()` method that returns a `BufferedSource`.

It looks like I can do something like:

```scala
val bufferedSource= Source.fromURL('https://explore.paulmatthews.dev/api/random/transactions?data_format=csv&amount=5')
```

To get an instance of the Buffered Source class which will contain the data I need!

## Code

```scala
def getTransactionStringFromAPI(amount: Int): Array[String] = {
    val url = s"https://explore.paulmatthews.dev/api/random/transactions?data_format=csv&amount=$amount"
    val bufferedSource = Source.fromURL(url)
    val stringResponse = bufferedSource.mkString
    bufferedSource.close
    stringResponse.split("\n").drop(1)
  }
```

A couple of additional pieces I added here. According to the docs **I am responsible for closing the buffered source**. Which I did on line 5.

I returned the split of `stringResponse` on the newline character `\n` after dropping the first row of the data which contains the header row of: `name,email,type,amount` which I don't want to include in my topic.