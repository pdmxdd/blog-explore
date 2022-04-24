---
title: "Challenge Three"
weight: 105
date: 2022-04-23
---

```md
## Challenge #3 - Complete the decoder challenge from the application in Scala
```

## Decoder Challenge

```md
Many years ago, the perfect cookie recipe was created and then lost... until now. An encrypted version was discovered
and it is up to you to recover the recipe so that the perfect cookie can be created once again.

The recipe is encrypted using a simple substitution cipher. For example, the encoded string `hgiikf` can be decoded to `butter`:

encoded -> decoded
      h -> b
      g -> u
      i -> t
      i -> t
      k -> e
      f -> r

The cipher is provided for you in a variable named `ENCODING`. 

## Challenge #1: Decode a string

Implement a function named `decodeString` that takes an encoded string and returns the decoded value (`hgiikf` is decoded to `butter`).

## Challenge #2: Decode an Ingredient

Implement a function named `decodeIngredient` that takes a line from the recipe and returns a new Ingredient (a class already defined for you).
The `#` sign delimits the encoded amount and the description of an ingredient. For example, the line `8 vgl#hgiikf` would return an Ingredient 
with an amount of `1 cup` and a description of `butter`. 

## Challenge #3: Decode the entire recipe

In the `main` method, read all of the ingredients out of `secret_recipe.txt`, decode each ingredient (hopefully using the functions
you implemented above), and save the output into a new file named `decoded_recipe.txt`.
```

A fun challenge. It doesn't seem too difficult, and there are three options for solving it in Python, Scala, or Java. I can see the solution in my mind for Python already, but since I've been digging into Scala I'll solve it in that language first.

## Starter Code

```scala
import scala.collection.immutable.HashMap

/**
 * An ingredient has an amount and a description.
 * @param amount For example, "1 cup"
 * @param description For example, "butter"
 */
case class Ingredient(amount: String, description: String)

object SecretRecipeDecoder {
  val ENCODING: Map[String, String] = HashMap[String, String](
    "y" -> "a",
    "h" -> "b",
    "v" -> "c",
    "x" -> "d",
    "k" -> "e",
    "p" -> "f",
    "z" -> "g",
    "s" -> "h",
    "a" -> "i",
    "b" -> "j",
    "e" -> "k",
    "w" -> "l",
    "u" -> "m",
    "q" -> "n",
    "n" -> "o",
    "l" -> "p",
    "m" -> "q",
    "f" -> "r",
    "o" -> "s",
    "i" -> "t",
    "g" -> "u",
    "j" -> "v",
    "t" -> "w",
    "d" -> "x",
    "r" -> "y",
    "c" -> "z",
    "3" -> "0",
    "8" -> "1",
    "4" -> "2",
    "0" -> "3",
    "2" -> "4",
    "7" -> "5",
    "5" -> "6",
    "9" -> "7",
    "1" -> "8",
    "6" -> "9"
  )

  /**
   * Given a string named str, use the Caeser encoding above to return the decoded string.
   * @param str A caesar-encoded string.
   * @return
   */
  def decodeString(str: String): String = {
    // todo: implement me
    "1 cup"
  }

  /**
   * Given an ingredient, decode the amount and description, and return a new Ingredient
   * @param line An encoded ingredient.
   * @return
   */
  def decodeIngredient(line: String): Ingredient = {
    // todo: implement me
    Ingredient("1 cup", "butter")
  }

  /**
   * A program that decodes a secret recipe
   * @param args
   */
  def main(args: Array[String]): Unit = {
    // TODO: implement me
  }
}
```

Ok. Quite a bit of starter code. 

The HashMap for conversions has already been built. So I should really just need to use the hashmap to decode whatever is provided.

### Encrypted Recipe

They also provide a `secret_recipe.txt` which I assume is the target for our decoder here. However, that will only happen after the initial units are up to par.

```txt
8 vgl#hgiikf
8 vgl#xyfe hfntq ogzyf, lyvekx
8 vgl#zfyqgwyikx ogzyf
4#kzzo
8 ikyolnnq#jyqawwy
4 8/4 vglo#nyiukyw
4 vglo#pwngf
8/4 ikyolnnq#oywi
8 ikyolnnq#hyeaqz onxy
8 ikyolnnq#hyeaqz lntxkf
84 ngqvko#vsnvnwyik vsalo
8#2-ngqvk uawe vsnvnwyik hyf
8 8/4 vglo#vsnllkx qgio

```

This should be fun and I'll have to learn some new things about Scala on the way.

## Challenge One: Decode a string

Let's walk before we run. First up, decode a string.

### Tests

The unit test already exists:

```scala
  describe("Testing decode_string") {
    it("can decode a string") {
      assert(SecretRecipeDecoder.decodeString("8 vgl") === "1 cup")
    }
  }
```

### Starter

```scala
  def decodeString(str: String): String = {
    // todo: implement me
    "1 cup"
  }

```

So they are encouraging test driven development here and have already done the first couple of steps:

1. write test
1. write unit fixture
1. fail test
1. provide the least amount of implementation to pass test **<-- we are here**
1. refactor solution
1. rerun tests to ensure they pass

### Refactor Solution

```scala
  def decodeString(str: String): String = {
    str.split("").map(ENCODING.getOrElse(_, " ")).mkString;
//    "1 cup"
  }
```

The plan was to split the string into a string array, then map to create a new string array with the conversion values, and then covert that back into a string.

### Retest Result: PASS!

That does it.

{{% notice note %}}
I originally got caught up when I just did `ENCODING.get`, not realizing it was going to return an `Option` **and** I tried to use `.toString` instead of `mkString`. I played with `.getOrElse` already so that was quite easy, and then simply had to google for the `.toString` preference IntelliJ told me about `mkString` and it was off to the races.
{{% /notice %}}

However, since I'm still new to `Scala` I'm going to add some new tests, just to give myself more confidence in the solution I have written, and give me a taste of writing tests. Which is always a good idea, but Scala pushes you in that direction by the very language they use, they call the methods you write Units.

### More Tests

I added the following three tests to the grouping created by the `describe()` block: 

```scala
    it("can decode 'hgiikf' to 'butter'") {
      assert(SecretRecipeDecoder.decodeString("hgiikf") === "butter")
    }
    it("can decode 'lygw' to 'paul'") {
      assert(SecretRecipeDecoder.decodeString("lygw") === "paul")
    }
    it("can decode '2 laqio' to '4 pints") {
      assert(SecretRecipeDecoder.decodeString("2 laqio") === "4 pints")
    }
```

### Result: PASS!

The syntax for writing tests using `scalatest` is very similar to `jest` with `Node`.

I am very comfortable with testing frameworks that use:

- `describe()`
- `it()`
- `assert()`

## Challenge Two: Decode an ingredient

### Test

```scala
  describe("Testing decode_ingredient") {
    it("can decode an ingredient") {
      val expected = Ingredient("1 cup", "butter")
      val actual = SecretRecipeDecoder.decodeIngredient("8 vgl#hgiikf")
      assert(actual.amount === expected.amount)
      assert(actual.description === expected.description)
    }
  }
```

### Starter

```scala
  def decodeIngredient(line: String): Ingredient = {
    // todo: implement me
    Ingredient("1 cup", "butter")
  }
```

Additionally a class has been defined:

```scala
case class Ingredient(amount: String, description: String)
```

Putting it together the `decodeIngredient()` method takes an encoded line like `8 vgl#hgiikf` and should return an Ingredient object like `Ingredient("1 cup", "butter").

Pretty straightforward especially since we already have the `decodeString()` method.

### My Solution

```scala
  def decodeIngredient(line: String): Ingredient = {
    val ingredient_as_list = line.split("#").map(ENCODING.getOrElse(_, " "))
    Ingredient(ingredient_as_list(0), ingredient_as_list(1))
  }
```

{{% notice note %}}
I was not expecting to access the ingredient list with parenthesis `()` and an index. That's a bit different than the square brackets I'm used to `[]`, but a simple google search for the scala docs put me right.
{{% /notice %}}

### Result: PASS!

A working solution.

However, I'm just duplicating the work I performed in `decodeString()` and am not using it. I need to fix that...

```scala
  def decodeIngredient(line: String): Ingredient = {
    val ingredient_as_list = line.split("#").map(decodeString)
    Ingredient(ingredient_as_list(0), ingredient_as_list(1))
  }
```

### Result: PASS!

I wouldn't say it's a great solution, but I don't know Scala well enough to know how to achieve things the scala way.

I might play with this more in the future, but will consider it done for now.

## Challenge Three: Decode the entire recipe

Ok no provided tests, and this last section should exist in the `main()` method according to the readme:

```md
In the `main` method, read all of the ingredients out of `secret_recipe.txt`, decode each ingredient (hopefully using the functions
you implemented above), and save the output into a new file named `decoded_recipe.txt`.
```

### My Solution

```scala
  def main(args: Array[String]): Unit = {
    val bufferedSource = Source.fromFile("src/main/resources/secret_recipe.txt")
    val lines = bufferedSource.getLines
    val decodedLines = lines.map(decodeString)
    val write_file = new File("src/main/resources/decoded_recipe.txt")
    val print_writer = new PrintWriter(write_file)
    decodedLines.foreach(print_writer.println)
    print_writer.close()
    bufferedSource.close()
  }
```

Not going to lie, that one took me a good half hour. Mainly getting Java file I/O operations to work...