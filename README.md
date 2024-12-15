# 🎄 Advent of Code

![GitHub](https://img.shields.io/github/license/mpichler94/advent-of-code-kotlin)
![Static Badge](https://img.shields.io/badge/Kotlin-2.1.0-blue)
![womm](https://cdn.rawgit.com/nikku/works-on-my-machine/v0.2.0/badge.svg)
![GitHub last commit (by committer)](https://img.shields.io/github/last-commit/mpichler94/advent-of-code-kotlin)

Solutions for [Advent of Code](https://adventofcode.com/) in [Kotlin](https://kotlinlang.org).

---

## Template setup

### Create your repository

1. Open [the template repository](https://github.com/mpichler94/advent-of-code-kotlin-template) on GitHub.
2. Click [Use this template](https://github.com/mpichler94/advent-of-code-kotlin-template/generate) and create your repository.
3. Clone your repository on your computer.

### Setup your Build Environment

You need a working development environment to write and run Kotlin code.
Reference the [Kotlin Docs](https://kotlinlang.org/docs/getting-started.html) for more information.

### Setup the Library

This library will get the puzzle input and example data from the website and also submit the results. 

Puzzle inputs are different for each user, so the library needs your session token to get the puzzle inputs for your specific user.
You can get the session token from your browser cookies after you are logged in on https://adventofcode.com. 
Check this [post from Stackexchange](https://superuser.com/a/1114501) for help.
Then you need to create a file with path `~/.config/adventofcode/token` containing only the session token.
On Windows, the path will be `C:\User\<username>\.config\adventofcode\token`.

## Usage

To use the library, implement a class containing the solution for a puzzle and create a `Day` object in the `main` function.
This is shown in the following snippet:
```kotlin
class PartA : PartSolution() {
    lateinit var numbers: List<Int>

    override fun parseInput(text: String) {
        numbers = text.trim().split("\n").map { it.toInt() }.toList()
    }

    override fun getExampleAnswer(): String {
        return "7"
    }

    override fun compute(): String {
        return countIncreases().toString()
    }

    private fun countIncreases(): Int {
        return numbers.windowed(2).map { it[1] > it[0] }.count { it }
    }
}

class PartB : PartA() {
    ...
}

fun main() {
    Day(2021, 1, PartA(), PartB())
}

```

Reference the API documentation of the `PartSolution` and `Day` classes for more information.

Each solution defines its own main class and can be executed directly from the editor with an IDE action.
[This link](https://www.jetbrains.com/help/idea/running-applications.html#run-from-editor) describes how to run an application in IntelliJ.

> [!TIP]
> This template also contains helpers for common puzzle problems located in the `at.mpichler.aoc.helpers` package.
> They are not required by the library itself, and you may delete or modify these to your preference.

## Advanced Use Cases

### Customize the sample input

The library will attempt to automatically detect and download the sample input for the puzzle.
However, it only searches for the first code block on the Advent of Code puzzle website, and thus it may be incorrect.
You can instead provide the puzzle input yourself by overriding the `getExampleInput()` method in the `PartSolution`.

### Add additional test cases

The library will only run your sample on the puzzle input, if it produces the example answer with the example input.
You may also want to add additional test cases to verify your solution.
By overriding the `tests()` method as shown below you can provide additional test cases which are checked before running your solution on the input.

```kotlin
override fun tests(): Sequence<Test> {
    return sequence {
        yield(Test("620080001611562C8802118E34", 12, "Operator packet(v3) with 2 subPackets"))
        yield(Test("C0015000016115A2E0802F182340", 23, "Operator packet"))
        yield(Test("A0016C880162017C3686B18A3D4780", 31, "Multiple nested operator packets"))
    }
}
```

The method is supposed to return a sequence of `Test` instances.
Construct the `Test` by providing the input, expected result and a name in this order.

## Internal Details

The puzzle input is downloaded once for each puzzle, when the corresponding `Day` function is called.
The input is then stored in `~/.config/adventofcode/{token}/yyyy_dd_input.txt`.
The example input is also downloaded and stored in this folder, but it may be incorrect since we only search for the first code block on the Advent of Code website.
All answers produced by your solution are also stored, and each unique answer is only submitted once.

