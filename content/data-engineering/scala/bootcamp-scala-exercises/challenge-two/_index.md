---
title: Challenge Two
weight: 100
date: 2022-04-23
---

## Instructions

```md
## Test #2: Implement code in each ChallengeProblems.scala to pass the tests

In the /exercises/ChallengeTests package, you'll find 
tests for every challenge in ChallengeProblems.scala. Implement code with passing tests before
moving on to the next challenge. There are 12 challenges total. 

*Note- Make sure to name each function with the name you find in the tests
*Additional Note - Uncomment test cases one at a time to run the tests
```

Now we're speaking my language, time to write some code to pass some tests.

## Test One

```scala
describe("Challenge One") {
  it("Checks if the string returned is the same as the string passed in"){
    val input: String = "Hello!"
    val expected: String = "Hello!"

    val actual = ChallengeProblems.sameString(input)
    assert(expected === actual)
  }
}
```

### My Solution

```scala
def sameString(someString: String): String = {
    someString
}
```

### Result: PASS

I did not know how to create methods in `scala`.

These feel like a hybrid they've got the `def` keyword I am familiar with from `python`, but required type declarations like `Pydantic`, `Java`, or `Rust`, they write like `Node` functions, they don't require a `return` statement like `Rust`. The combination of a bunch of pieces from other languages I've worked with. Overall I already feel pretty comfortable with them.

I can see how using this syntax would allow you to write very concise methods or functions that fit nicely into the functional paradigm.

## Test Two

```scala
  describe("Challenge Two") {
    it("Checks if the string returned is the same as the string passed in"){
      val expected: String = "Hello World!"

      val actual = ChallengeProblems.helloWorld()
      assert(expected === actual)
    }
  }
```

### My Solution

```scala
  def helloWorld(): String = "Hello World!"
```

### Result: PASS

## Test Three

```scala
  describe("Challenge Three") {
    it("Checks if list size is correct"){
      val input:List[Int] = List(1,2,3,4,5,6)
      val expected = 6
      val actual = ChallengeProblems.listSize(input)
      assert(expected === actual)
    }
  }
```

### My Solution

```scala
  def listSize(someList: List[Int]): Int = someList.size;
```

### Result: PASS

## Test Four

```scala
  describe("Challenge Four") {
    it("Checks if sum is correct"){
      val input:Int = 7
      val expected = 32
      val actual = ChallengeProblems.sumInts(input)
      assert(expected === actual)
    }
  }
```

Challenge note:

```scala
  /*
  4. Write a function that takes in an int and adds an int that you create within the function and returns the addition of the two together
  Note - Your variable must be a val and must be equal to 25
  Params - Int
  Returns - Int
   */
```

### My Solution

```scala
  def sumInts(someInt: Int): Int = {
    val addition = 25;
    someInt + addition;
  }
```

It could have been possible to solve the challenge with:

```scala
def sumInts(someInt: Int): Int = someInt + 25;
```

However, this would not have needed the inclusion of the variable with the `val` keyword. Which I had to look up the syntax.

## Test Five

```scala
  describe("Challenge Five") {
    it("Uses .map to uppercase everything") {
      val input = List("Scala", "is", "dope")
      val expected = List("SCALA", "IS", "DOPE")
      val actual = ChallengeProblems.upper(input)
      assert(expected === actual)
    }
  }
```

### My Solution

```scala
  def upper(someList: List[String]): List[String] = someList.map(_.toUpperCase())
```

### Result: PASS

## Test Six

```scala
  describe("Challenge Six") {
    it("Checks if filtered out values are correct") {
      val input = List(0,-3,13,25)
      val expected = List(0,13,25)
      val actual = ChallengeProblems.filterNegatives(input)
      assert(expected === actual)
    }
    it("Checks if all negatives, then should be an empty list") {
      val input = List(-3,-5,-6)
      val expected = List()
      val actual = ChallengeProblems.filterNegatives(input)
      assert(expected === actual)
    }
  }
```

Two test cases. Now we're getting serious.

### My Solution

```scala
  def filterNegatives(someList: List[Int]): List[Int] = someList.filter(_ > -1);
```

### Result: PASS

So far everything has been around some `scala` basics and really emphasizing the `functional` paradigm. I've already played with functional a fair amount with both `Node` and `Python`.

Looking at the standard library for `scala` it seems like there are many more methods available to create very specific functional solutions.

## Test Seven

```scala
  describe("Challenge Seven") {
    it("Checks if words with car in it are kept") {
      val input = List("racecar", "cardinal", "dancer")
      val expected = List("racecar", "cardinal")
      val actual = ChallengeProblems.containsCar(input)
      assert(expected === actual)
    }
  }
```

### My Solution

```scala
  def containsCar(someList: List[String]): List[String] = someList.filter(_.contains("car"));
```

### Result: PASS

## Test Eight

```scala
  describe("Challenge Eight") {
    it("Checks if sum of all ints is correct") {
      val input = List(0,23,4,-1,8)
      val expected = 34
      val actual = ChallengeProblems.sumList(input)
      assert(expected === actual)
    }
  }
```

### My Solution

```scala
  def sumList(someList: List[Int]): Int = someList.sum;
```

### Result: PASS

{{% notice note %}}
I originally wrote:
```scala
  def sumList(someList: List[Int]): Int = someList.reduceLeft(_ + _);
```
But IntelliJ told me to replace `.reduceLeft(_ + _)` with `.sum`. I like that. The method name `sum` is more clear than simply performing an aggregation with `reduce`.
{{% /notice %}}

## Test Nine

```scala
  describe("Challenge Nine") {
    it("Returns cat age from human age when passed an int") {
      val input = 3
      val expected = 12
      val actual = ChallengeProblems.catsAge(input)
      assert(expected === actual)
    }
  }
```

### My Solution

```scala
  def catsAge(someInt: Int): Int = someInt * 4;
```

### Result: PASS

## Test Ten

```scala
  describe("Challenge 10") {
    it("Returns the cat age from a human age when passed a Some") {
      val input: Option[Int] = Some(4)
      val expected: Option[Int] = Some(16)

      val actual = ChallengeProblems.catsAgeOption(input)
      assert(expected === actual)
    }

    it("Returns a None when passed a None") {
      val input: Option[Int] = None
      val expected: Option[Int] = None

      val actual = ChallengeProblems.catsAgeOption(input)
      assert(expected === actual)
    }
  }
```

### My Solution

```scala
  def catsAgeOption(someInt: Option[Int]): Option[Int] = {
    if (someInt.isEmpty) return None else return Some(someInt.get * 4);
  }
```

It's a little gross, but I think it will work.

### Result: PASS

### Refactor

```scala
      def catsAgeOption(someInt: Option[Int]): Option[Int] = if (someInt.isEmpty) return None else return Some(someInt.get * 4);
```

### Result: PASS

A little better, little worse. I don't know any of the `scala` best practices. Is it preferred to write one liners, or to break them up to make them readable?

I'll find out later I'm sure.

## Test Eleven

```scala
describe("Challenge Eleven") {
    it("Checks if minimum value in list is returned") {
      val input:List[Int] = List(1,-4,19,10,0)
      val expected = -4
      val actual = ChallengeProblems.minimum(input)
      assert(expected === actual)
    }
  }
```

### My Solution

```scala
  def minimum(someList: List[Int]): Int = someList.min;
```

### Result: PASS

## Test Twelve

```scala
  describe("Challenge Twelve") {
    it("Checks if minimum is returned") {
      val input:List[Option[Int]] = List(Some(1),Some(-4),Some(19),Some(10),Some(-3))
      val expected = Some(-4)
      val actual = ChallengeProblems.minimumOption(input)
      assert(expected === actual)
    }
    it("Returns a None when passed a None") {
      val input:List[Option[Int]] = List(None, None)
      val expected = None
      val actual = ChallengeProblems.minimumOption(input)
      assert(expected === actual)
    }
  }
```

### My Solution

```scala
  def minimumOption(someList: List[Option[Int]]): Option[Int] = {
    var theReturn: Option[Int] = None;
    for (possibleNum <- someList) {
      if (possibleNum.isDefined) {
        val num = possibleNum.get;
        if (num < theReturn) {
          theReturn = Some(num);
        }
      }
    }
    theReturn;
  }
```

### Result: Compile Error

```bash
overloaded method value < with alternatives:
  (x: Double)Boolean <and>
  (x: Float)Boolean <and>
  (x: Long)Boolean <and>
  (x: Int)Boolean <and>
  (x: Char)Boolean <and>
  (x: Short)Boolean <and>
  (x: Byte)Boolean
 cannot be applied to (Option[Int])
        if (num < theReturn) {
```

That makes sense I'm currently comparing an `Int` to an `Option[Int]`. I need to get the `Int` out of `theReturn`, but it may not exist. I can probably use `.getOrElse()`.

### Fix

```scala
  def minimumOption(someList: List[Option[Int]]): Option[Int] = {
    var theReturn: Option[Int] = None;
    for (possibleNum <- someList) {
      if (possibleNum.isDefined) {
        val num = possibleNum.get;
        if (num < theReturn.getOrElse(num + 1)) {
          theReturn = Some(num);
        }
      }
    }
    theReturn;
  }
```

### Result: PASS

Meh. The solution passes both tests. I don't like that it's a traditional `for` loop instead of some snazzy functional solution, but I couldn't wrap my head around how I could mold a functional method to arrive at this solution.