---
title: "Rabbit Hole: Morse Code"
weight: 110
date: 2022-04-24
---

About this time last year I taught a CodeCamp for Comcast. It included Node, HTML, CSS, DOM, Angular, Java, Springboot, SQL, and Hibernate.

When teaching that class I put together a morse code to english (and vice versa) encoding decoding project as a demo for my students. It originally lived completely on repl.it, but I just moved it to GitHub:

- [Repl.it project](https://replit.com/@lc_paul/Morse-Code-Translator)
- [GitHub Repo](https://github.com/pdmxdd/node-morse-code)

It wasn't focused on functional programming at all and was really just demonstrating programming fundamentals in JavaScript. After looking over the source code it looks like I snuck a couple of `.map()` methods in there!

I think I will rebuild it in `Scala` as it compliments the challenge I just completed.

## New Project

Uhh...

I created a hello-world project earlier with `sbt`. It didn't have maven, but I don't think this project will need anything outside of Scala's standard library so I'll just create it with `sbt`.

```bash
sbt new scala/scala3.g8
```

And then entered `morse-code` which created a new Scala 3 project directory in my home directory.

According to the Scala docs I can simply open the created `build.sbt` as a project in IntelliJ and it will configure the rest.

## First HashMap

Using the same syntax from the previous example I created:

```scala
import scala.collection.immutable.HashMap

@main def hello: Unit =
  println(morseCodeToLetter)

val morseCodeToLetter: Map[String, String] = HashMap[String, String] (
  ".-" -> "a",
  "-..." -> "b",
  "-.-." -> "c",
  "-.." -> "d",
  "." -> "e",
  "..-." -> "f",
  "--." -> "g",
  "...." -> "h",
  ".." -> "i",
  ".---" -> "j",
  "-.-" -> "k",
  ".-.." -> "l",
  "--" -> "m",
  "-." -> "n",
  "---" -> "o",
  ".--." -> "p",
  "--.-" -> "q",
  ".-." -> "r",
  "..." -> "s",
  "-" -> "t",
  "..-" -> "u",
  "...-" -> "v",
  ".--" -> "w",
  "-..-" -> "x",
  "-.--" -> "y",
  "--.." -> "z",
  "-----" -> "0",
  ".----" -> "1",
  "..---" -> "2",
  "...--" -> "3",
  "....-" -> "4",
  "....." -> "5",
  "-...." -> "6",
  "--..." -> "7",
  "---.." -> "8",
  "----." -> "9",
  ".-.-.-" -> ".",
  "--..--" -> ",",
  "..--.." -> "?",
  "-.-.--" -> "!",
  "...---..." -> "sos",
)
```

And got this output:

```bash
HashMap(. -> e, .... -> h, --. -> g, .-. -> r, -.... -> 6, --- -> o, .--. -> p, ..--.. -> ?, -.-. -> c, --.- -> q, ..-. -> f, .-- -> w, ---.. -> 8, ..- -> u, -. -> n, ----- -> 0, --..-- -> ,, -..- -> x, .---- -> 1, ...- -> v, - -> t, .- -> a, ....- -> 4, -.. -> d, .. -> i, -.-- -> y, --.. -> z, ...-- -> 3, ... -> s, ..... -> 5, ----. -> 9, -- -> m, .-.-.- -> ., ..--- -> 2, ...---... -> sos, -.- -> k, .-.. -> l, .--- -> j, -... -> b, --... -> 7, -.-.-- -> !)
```

## Second HashMap

Using the `.map()` method I should be able to easily reverse the keys and values to create a new HashMap in one concise line:

```scala
val letterToMorseCode: Map[String, String] = morseCodeToLetter.map((key, value) => value -> key)
```

And print it out as well in the main method:

```scala
@main def hello: Unit =
  println(letterToMorseCode)
```

Output:

```bash
HashMap(e -> ., 8 -> ---.., t -> -, ! -> -.-.--, a -> .-, 5 -> ....., m -> --, i -> .., p -> .--., 2 -> ..---, w -> .--, 3 -> ...--, k -> -.-, s -> ..., x -> -..-, 4 -> ....-, n -> -., . -> .-.-.-, 9 -> ----., j -> .---, y -> -.--, u -> ..-, f -> ..-., , -> --..--, v -> ...-, 6 -> -...., 1 -> .----, q -> --.-, b -> -..., g -> --., l -> .-.., 0 -> -----, ? -> ..--.., sos -> ...---..., c -> -.-., h -> ...., 7 -> --..., r -> .-., o -> ---, z -> --.., d -> -..)
```

Well that just worked like a charm.

## Testing (manually) it Out

I changed the main method to:

```scala
  // paul should be: .--. .- ..- .-..
  println("paul".split("").map(letterToMorseCode.getOrElse(_, "ERROR")).mkString(" "))
  // dog should be: -.. --- --.
  println("dog".split("").map(letterToMorseCode.getOrElse(_, "ERROR")).mkString(" "))
  // sos should be: ... --- ...
  println("sos".split("").map(letterToMorseCode.getOrElse(_, "ERROR")).mkString(" "))

  // .--. .- ..- .-.. should be: paul
  println(".--. .- ..- .-..".split(" ").map(morseCodeToLetter.getOrElse(_, "ERROR")).mkString)
  // -.. --- --. should be: dog
  println("-.. --- --.".split(" ").map(morseCodeToLetter.getOrElse(_, "ERROR")).mkString)
  // ... --- ... should be: sos
  println("... --- ...".split(" ").map(morseCodeToLetter.getOrElse(_, "ERROR")).mkString)
```

And got this output:

```bash
.--. .- ..- .-..
-.. --- --.
... --- ...
paul
dog
sos
```

Very nice. If I was more comfortable with Scala, I would have written unit tests. But that will come with more time in the language.

## GitHub Repo

Take a look at the [scala-morse-code GitHub repo](https://github.com/pdmxdd/scala-morse-code) to see the full code.