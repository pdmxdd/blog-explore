---
title: "Challenge Four"
weight: 115
date: 2022-04-25
---

```md
## Challenge #4: Complete Option either questions in the /practice directory. 
Make sure to run tests in the OptionEitherTests suite to ensure all tests pass. 
```

## Option Either 

Full tests:

```scala
class OptionEitherTests extends FunSpec {
  describe("dog age") {
    it("should return none when none is given") {
      val humanAge = None
      val expected = None

      val actual = OptionEither.dogAge(humanAge)
      assert(actual === expected)
    }

    it("should return correct dog age") {
      val humanAge: Option[Int] = Some(4)
      val expected = Some(28)

      val actual = OptionEither.dogAge(humanAge)
      assert(actual === expected)
    }
  }

  describe("Total Cost"){
    it("Should cost with tax, if a cost is provided"){
      val item: Item = Item("BoomBox", Some(100))
      val expected = Some(107)

      val actual = OptionEither.totalCost(item)
      assert(actual === expected)
    }

    it("Should return None if no tax is provided"){
      val item: Item = Item("Free Tickets", None)
      val expected = None

      val actual = OptionEither.totalCost(item)
      assert(actual === expected)
    }
  }

  describe("Average Temperatures List") {
    it("Should return the average temprature"){

      val temperatures: List[WeatherStation] = List(WeatherStation("Baltimore", Some(10)), WeatherStation("Seattle", Some(40)), WeatherStation("St. Louis", None), WeatherStation("Chicago", Some(20)), WeatherStation("Omaha", None), WeatherStation("Atlanta", None), WeatherStation("LA", Some(30)))
      val expected = Some(25)

      val actual = OptionEither.averageTemperature(temperatures)
      assert(actual.isDefined)
      assert(actual.get === expected.get)
    }

    it("Should return None when the list doesn't contain any"){
      val temperatures: List[WeatherStation] = List(WeatherStation("St. Louis", None), WeatherStation("Omaha", None), WeatherStation("Atlanta", None))

      val actual = OptionEither.averageTemperature(temperatures)
      assert(actual.isEmpty)
    }
  }
```

Starter Code:

```scala
case class Item(description: String, price: Option[Int])

case class WeatherStation(name: String, temperature: Option[Int])

object OptionEither {
  /*
    Returns age of a dog when given a human age.
    Returns None if the input is None.
  */
  def dogAge(humanAge: Option[Int]): Option[Int] = ???

  /*
    Returns the total cost af any item.
    If that item has a price, then the price + 7% of the price should be returned.
  */
  def totalCost(item: Item): Option[Double] = ???

  /*
    Given a list of weather temperatures, calculates the average temperature across all weather stations.
    Some weather stations don't report temperature
    Returns None if the list is empty or no weather stations contain any temperature reading.
   */
  def averageTemperature(temperatures: List[WeatherStation]): Option[Int] = ???
}
```

## Dog Age

### Tests

```scala
  describe("dog age") {
    it("should return none when none is given") {
      val humanAge = None
      val expected = None

      val actual = OptionEither.dogAge(humanAge)
      assert(actual === expected)
    }

    it("should return correct dog age") {
      val humanAge: Option[Int] = Some(4)
      val expected = Some(28)

      val actual = OptionEither.dogAge(humanAge)
      assert(actual === expected)
    }
  }
```

Pretty straightforward.

If the argument is `None` it should return `None`. Else it should return argument * 7.

### My Solution

```scala
  def dogAge(humanAge: Option[Int]): Option[Int] = {
    if (humanAge.isEmpty) return None

    Some(humanAge.get * 7)
  }
```

### Result: PASS

Similar to the catAge exercise, but this time an Option is included.

## Total Cost

### Tests

```scala
  describe("Total Cost"){
    it("Should cost with tax, if a cost is provided"){
      val item: Item = Item("BoomBox", Some(100))
      val expected = Some(107)

      val actual = OptionEither.totalCost(item)
      assert(actual === expected)
    }

    it("Should return None if no tax is provided"){
      val item: Item = Item("Free Tickets", None)
      val expected = None

      val actual = OptionEither.totalCost(item)
      assert(actual === expected)
    }
  }
```

Similar to the previous problem if the Optional is empty return None, if not return the actual value wrapped in an Optional.

### My Solution

I think I'm getting better at Scala.

```scala
  def totalCost(item: Item): Option[Double] = Some(item.price.getOrElse(return None) * 1.07)
```

### Result: PASS

Since they didn't give me a function body curly bracket I think they are leading me to not including those when possible.

I'll refactor the previous solution as well.

### Refactor Dog Age

```scala
  def dogAge(humanAge: Option[Int]): Option[Int] = Some(humanAge.getOrElse(return None) * 7)
```

### Result: PASS

In a previous article I commented about not knowing the Scala way, I presumed it was functional in nature. Based on the design of this set of exercises I would say a purely functional solution is preferred.

From what I can surmise from data engineering this makes a ton of sense. A data engineering solution is built for a massive amount of data: **terabytes**, **petabytes** possibly even more. 

Data on such a scale that it can only be conceivably worked through by using a **distributed** computing strategy. Data needs to be broken down and given to a network of much smaller computers (nodes as they are often called) and each node performs the function on their subset of data in **parallel**. Allowing a massive volume of data to be processed in a relatively quick amount of time, and the time can be further reduced by **scaling even further horizontally** by the introduction of even more nodes.

In addition to processing speed over the distributed network you also get data **redundancy** as most of the nodes will be configured to share copies with each other in case one node becomes corrupted or goes dark.

{{% notice note %}}
After coming to this realization I'd like to do two things:
1. practice solving coding problems in a purely functional way (I'll probably use leetcode, or exercism)
1. setup a practice system that performs parallel processing across multiple machines to transform or analyze a large set of data (possibly scale the system)

I'll need to keep my eye out for large and interesting data sets. Or just generate a dummy data set.
{{% /notice %}}

## Average Temperature Lists

### Tests

```scala
  describe("Average Temperatures List") {
    it("Should return the average temprature"){

      val temperatures: List[WeatherStation] = List(WeatherStation("Baltimore", Some(10)), WeatherStation("Seattle", Some(40)), WeatherStation("St. Louis", None), WeatherStation("Chicago", Some(20)), WeatherStation("Omaha", None), WeatherStation("Atlanta", None), WeatherStation("LA", Some(30)))
      val expected = Some(25)

      val actual = OptionEither.averageTemperature(temperatures)
      assert(actual.isDefined)
      assert(actual.get === expected.get)
    }

    it("Should return None when the list doesn't contain any"){
      val temperatures: List[WeatherStation] = List(WeatherStation("St. Louis", None), WeatherStation("Omaha", None), WeatherStation("Atlanta", None))

      val actual = OptionEither.averageTemperature(temperatures)
      assert(actual.isEmpty)
    }
  }
```

Final test suite.

`10 + 40 + 20 + 30` is `100` divided by `4` is `25`. So assuming some of the WeatherStations have a value you need to aggregate them and divide them by only the stations that had values to calculate the average. If **all** of the weather stations lack values you should return None.

### My Solution

```scala
  def averageTemperature(temperatures: List[WeatherStation]): Option[Int] = {

    val filteredStations = temperatures.filter(_.temperature.isDefined)
    val filteredStationsAmount = filteredStations.size
    if (filteredStationsAmount == 0) {
      return None
    }
    val totalTemp = filteredStations.map(_.temperature.get).sum
    println(totalTemp)
    Some(totalTemp / filteredStationsAmount)

  }
```

A readable solution. If the tests pass I'll try to make it more concise.

### Result: PASS

### Refactor

```scala
def averageTemperature(temperatures: List[WeatherStation]): Option[Int] = if (temperatures.count(_.temperature.isDefined) > 0) return Some(temperatures.filter(_.temperature.isDefined).map(_.temperature.get).sum / temperatures.count(_.temperature.isDefined)) else return None
```

Will this nonsense pass the tests?

### Result: PASS

{{< hugo-giphy-shortcode "11FiDF2fuOujPG" >}}

It works and I understand it!

But to quote Jeff Goldblum as Dr. Ian Malcom from Jurassic Park:

> Yeah, yeah, but your scientists were so preoccupied with whether or not they could that they didn't stop to think if they should.

I think it's cool that you can keep transforming data to get very specific results, but it's not always readable. Readable code is maintainable code. I'm not quite sure I understand where the line exists.

Upon further reflection I believe you can achieve functional programming without creating one line Frankensteins. It's something I'm going to have to keep working towards moving forward.