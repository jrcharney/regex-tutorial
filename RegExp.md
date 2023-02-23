# Regular Expression

Written by Jason Charney

**Regular Expression** (a.k.a. RegEx, RegExp, or RE), is a sequence of characters that specify a search pattern in text.  Typically, RegExes are used to find and/or replace text that matches patterns.

## Summary

In this document, I will describe how to use regular expression, how it works, and a few concepts you probably didn't know were the inspiration for its functional design.

## Table of Contents

- [Regular Expression](#regular-expression)
  - [Summary](#summary)
  - [Table of Contents](#table-of-contents)
  - [How to *actually* learn RegEx](#how-to-actually-learn-regex)
    - [What is Finite Automata?](#what-is-finite-automata)
    - [How Computers Process Data from 01000001 to 01011010](#how-computers-process-data-from-01000001-to-01011010)
  - [Creating Regular Expressions in JavaScript](#creating-regular-expressions-in-javascript)
    - [Flags](#flags)
  - [Special Characters Types](#special-characters-types)
    - [Character Classes](#character-classes)
      - [ASCII Table](#ascii-table)
      - [Escape Sequences](#escape-sequences)
      - [Character Ranges](#character-ranges)
      - [Special Character Classes](#special-character-classes)
    - [Quantifiers](#quantifiers)
      - [Lazy Quantifiers](#lazy-quantifiers)
      - [Examples of Quantifiers in Action](#examples-of-quantifiers-in-action)
    - [Groups and Backreferences](#groups-and-backreferences)
      - [Groups](#groups)
      - [Backreferences](#backreferences)
      - [Examples of Groups and Backreferences](#examples-of-groups-and-backreferences)
    - [Assertions](#assertions)
      - [Boundary-type Assertions](#boundary-type-assertions)
        - [Examples of Boundary-type Assertions](#examples-of-boundary-type-assertions)
      - [Look Assertions](#look-assertions)
        - [Examples of Look Assertions](#examples-of-look-assertions)
      - [Conditional Assertions](#conditional-assertions)
    - [Unicode Property Escapes](#unicode-property-escapes)
      - [UPE Syntax](#upe-syntax)
      - [A Short List of Unicode Properties](#a-short-list-of-unicode-properties)
  - [JavaScript RegExp properties and methods](#javascript-regexp-properties-and-methods)
    - [`RegExp`](#regexp)
      - [RegExp constructor](#regexp-constructor)
      - [RegExp static properties](#regexp-static-properties)
      - [RegExp instance properties](#regexp-instance-properties)
      - [RegExp instance methods](#regexp-instance-methods)
    - [`String`](#string)
      - [String instance methods](#string-instance-methods)
  - [About the Author](#about-the-author)
  - [Sources](#sources)

## How to *actually* learn RegEx

I was given a list of points to go over because this is a Computer Science assignment. There's just one thing: I **AM** a computer scientists.  I got my BSCS in 2007, and a lot of time we learned how to do regular expressions.

To understand what regular expressions are, let's go over a few concepts from finite automata and how computer science is more about mathematics than it is computers.

### What is Finite Automata?

An **automata** (plural of "automation") is a machine, typically theoretical.

**Finite automata** (also called **finite state machines**, FSM, or FA) are mathematical computation models. These models of computation describe the output of a mathematical function given an input. Take note of this sentence.

While these models are theoretical, there are physical automata.  Wind-up toys or music boxes are perfect examples of what automata are.  The input on a music box is that metal scroll that plucks the comb that makes each note, and the output is the audible sound that we perceive as music. While the music box does require energy to wind up the spring, that input is secondary and often neglected for these sorts of examples.

When you learn about how random-access memory (RAM) works, then time becomes a factor, but this document will not go into such concepts.

For now, let's keep it simple.

You probably hadn't noticed that throughout your education in mathematics, you've been using finite automata to do homework with.  The simple arithmetic operators you've been using called *operators* have relied on input variables to produce an output.  In the beginning, these input variables were numbers. As you got older, you were introduced to the concept of algebra and learned how to reverse-engineer these variables to "solve for x".

**Finite automata are mathematical models of computation that describe the output of a mathematical function given an input.  Regular Expression is the *language* of finite automata.**

Just like any language, they are made of words and words are simply a group of *symbols* put together to describe a concept or idea.  The set of symbols used to make those words is called an **alphabet** and the letters that make up that alphabet are called **symbols** or **elements** or even **atoms**.  The words made of those symbols are called **strings**.  To construct a word, you need to string together symbols from a finite alphabet.

### How Computers Process Data from 01000001 to 01011010

So how does the computer understand what words are?  At the *bare-metal* level of computing, it doesn't.  A computer is just a box of switches...teeny tiny switches thanks to miniaturization techniques in computer manufacturing.  Each one of these switches has a **binary** state, meaning it has two states zero (off) and one (on).  Maybe "off-and-on" aren't the best way to describe it. More like "low-and-high" because even in an "off" state, there is still an electrical current passing through each of those switches.  These switches are actually **semiconductors** as they are electrical circuits made of *semi-conductive* materials of silicon and metal.

Think of semiconductors as a neuron cell inside of a brain.  On its own, it doesn't do a whole lot. But put hundreds, thousands, millions, or billions of those cells together and those neurons can operate collectively as one machine.  The simple theoretical study of these cellular machines is what makes **cellular automata** (CA) possible.

An example of this would be if there was a room full of mouse traps (neurons) with ping pong balls (messenger signals or inputs that could also be outputs) on them.  If you threw a ping pong ball in this room and it triggered one trap, it would trigger other traps until most of the traps were triggered.  That one ping pong ball created a chain of events.  This is how signals and messages work.  A computer takes in (processes) signal inputs and puts out (produces) signal outputs.

The **binary alphabet** is simply $\Sigma = \{0,1\}$. The **bit**, or **binary digit**, is the unit of information that represents either a $0$ or $1$ in an element $e$.  A string of four bits is called a **nibble** and a string of eight bits make up a **byte**.

The next level up from the bare-metal of the computer is the **machine language** that processes all those bytes.  A byte on simple 8-bit computer would have a value between $00000000$ and $11111111$. Today, most consumer computers are 64-bit machines because the set of symbols that humans can use has grown much larger to include international characters.  Even so, it is common to group bytes into nibbles and represent them in **hexadecimal** notation.

Hexadecimal is a number system that has a value between $0$ and $\text{F}$, roughly equivalent to between zero and fifteen in decimal system.  Humans have used the *decimal* system for millennia because biologically, the majority of us have ten fingers.  If we had four fingers, our society may have been built on an *octal* system.  In fact, early computers used octal (an 8-digit number system) because at the time, computer engineers represented bytes with 7 bits instead of 8 bits which could be represented by two hexadecimal nibbles.

| Bin  | Oct | Dec | Hex |
|------|-----|-----|-----|
| 0000 |  0  |  0  |  0  |
| 0001 |  1  |  1  |  1  |
| 0010 |  2  |  2  |  2  |
| 0011 |  3  |  3  |  3  |
| 0100 |  4  |  4  |  4  |
| 0101 |  5  |  5  |  5  |
| 0100 |  6  |  6  |  6  |
| 0111 |  7  |  7  |  7  |
| 1000 | 10  |  8  |  8  |
| 1001 | 11  |  9  |  9  |
| 1010 | 12  | 10  |  A  |
| 1011 | 13  | 11  |  B  |
| 1100 | 14  | 12  |  C  |
| 1101 | 15  | 13  |  D  |
| 1100 | 16  | 14  |  E  |
| 1111 | 17  | 15  |  F  |

Later on in this document, we will show the ASCII table which will show the advantage of using hexadecimal nibbles, and why those nibbles have since been expanded in Unicode to allow for the inclusion of other symbols from other alphabets and now includes emoji.

For now, know that a **character** is represented by a byte which is made of at least two nibbles.

This emphasis on nibbles, was what made the next level of computing possible: assembly language.

**Assembly language** is a *low-level* programming language that would be converted into machine language to execute bare-metal actions.  Almost all dialects of assembly language use a program called an **assembler** to convert *source code* into a *binary* which is either a data object (called *modules*) or library (also called an *archive*) or executable program.

If you tried using the `cat` command on a binary, you would see a bunch of garbled text that could mess up the terminal.  If that happens, you just simply close the terminal and open a new one or in worst-case reset the computer.  This is why it is important to use file extensions.  

Assembly language can be difficult, you need to be vigilant for memory leaks and errors.  It is often a good ideal for people who want to learn assembly to practice it in a "sandbox" environment using a virtual machine that mimics a computer within your computer.

Some source code needs to be linked together to form a complete program.  The program that does this is called a **linker**.  The fact that we can link modules together to form programs with existing code is why making reusable code a fundamental part of computer programming.  You don't need to rewrite that code; you just borrow data from an archive file and reuse it as part of the program you are writing.

The next level up from Assembly Language is the **high-level programming language**. Typically C, C++, Java, or Rust can be considered a "high-level" programming language. These languages are typically *compiled* using a program called a **compiler**.  The compiler typically does compiling and the assembly in one step, though some compilers to allow you the option to do these steps separately.  If you are simply writing a small program, the compiler can also do the linking.

Finally, there is the *very high-level* programming language or **scripting language**. Typically these programming languages are considered "type safe" because you can write an error somewhere and the program will not run until it is fixed.  JavaScript is a scripting language. It has plenty of distance from the bare-metal that even if you encounter memory leaks or data errors, these won't harm the computer.

A feature that most scripting languages have, and some high-level languages as well, are built in regular expression.  In C and C++, the library `regex.h` (typically found at `/usr/include/regex.h`), can be added as an `#include` statement to allow for regular expressions to be used.  In JavaScript we can create them without needing to include a library.

## Creating Regular Expressions in JavaScript

While most languages should have the ability to process Regular Expression, the best languages emphasize the use of regular expression do do things. Perl, Ruby, Python, and JavaScript especially.  Although most of these languages rely on a library written in C.

JavaScript is likely the easiest entry into regular expression because of its accessibility. You can try it out in your console.

There are two ways to create a regular expression in JavaScript:

1. **As a regexp literal**, which consists of a pattern enclosed between forward slashes.
2. **As an instance of a `RegExp` object**, with the pattern in quotes.

```javascript
// An example of a regexp literal
const literal_re = /^[a-z]+$/;

// An example of a regexp instance
const instance_re = new RegExp("^[a-z]+$");
```

The literal form is common and meant for speed, while the object instance form is good when using forward slashes as the delimiter is not ideal, like if you need to escape the forward slash.  In some languages, you can define your own delimiter as a substitute for the slash.

Most Regexp generally consist of two parts:

- What to match
- How many times to match it

The *what* seems pretty obvious. Typically you want to match a set of characters.  However, JavaScript does offter the ablity to match specific strings.  We can go over that part later.

Some characters have special meaning in an RE because of where they are located in a string. For instance, the **carat** and the **dollar sign** indicate that a string should be at the start or end of a line.  If they are used together, it means that the line should consist of only the characters between those two characters. So `/^$/` means to match empty lines. But what if you want to match the exact opposte of that?  If you want to match a line that has any character in it, you would use `/^.*$/`.

| Char. | Where to use it | What it means |
|-------|-----------------|---------------|
| `^`   | The first character in a RE pattern | The string should be at the beginning of a line. |
| `$`   | The last character in a RE pattern | The string should be at the end of a line. |

The carat and the dollar sign are two types of *assertions* which we will describe again later.

### Flags

There is a short list of flags you can use at the end of a `RegExp`

| Flag | Property            | Indicates |
|------|---------------------|-----------|
| `d`  | **hasIndices**      | the result of a regular expression match should contain the start and end indices of the substrings of each capture group |
| `g`  | **global**          | test against all possible matches in a string. |
| `i`  | **caseInsensitive** | ignore case |
| `m`  | **multiline**       | Test all lines, not just one line |
| `s`  | **dotAll**          | Specify that the dot (`.`) should match new line characters. |
| `u`  | **unicode**         | Enable unicode matching. This allows `\u` and `\p` to work. |
| `y`  | **sticky**          | Allow the `lastIndex` property to track what the most recent index was in an array of matches the RegExp found. |

## Special Characters Types

There are five types of special characters in regular expression.

- **Character classes** which are groups of characters to be matched. These define the *what* to match. This includes escape sequences and character ranges.
- **Quantifiers** which describe *how many times* a character or specific class of characters should occur.
- **Groups and backreferences** which can be created to define and reuse matching sequencies.
- **Assertions** which define *where* matching a sequence should occur.
- **Unicode property escapes** which are probably the least use, but we will mention them anyway.

### Character Classes

#### ASCII Table

To understand how character classes work, let's look at the ASCII table.  The **ASCII table** are the first 128 character in Unicode, and the fundamental characters used by computers.  If you are on a Linux or UNIX system, you can type `man ascii` to see the table.  Each character initially represented 7 bits then it was expanded to 8 bits.  Thanks to Unicode and the growth of computer systems from 8-bit, to 16-bit, to 32-bit, to 64-bit systems over the last 40 years, the amount of bits per character has grown to 32-bits. But most of us will still use UTF-8.

This table also shows what the typical **escape sequences** are and why we can use **character ranges** in regular expression.

```text
Oct   Dec   Hex   Char                      │ Oct   Dec   Hex   Char  │ Oct   Dec   Hex   Char    │ Oct   Dec   Hex   Char    
────────────────────────────────────────────┼─────────────────────────┼───────────────────────────┼────────────────────────
000   0     00    NUL '\0' (null character) │ 040   32    20    SPACE │ 100   64    40    @       │ 140   96    60    `
001   1     01    SOH (start of heading)    │ 041   33    21    !     │ 101   65    41    A       │ 141   97    61    a
002   2     02    STX (start of text)       │ 042   34    22    "     │ 102   66    42    B       │ 142   98    62    b
003   3     03    ETX (end of text)         │ 043   35    23    #     │ 103   67    43    C       │ 143   99    63    c
004   4     04    EOT (end of transmission) │ 044   36    24    $     │ 104   68    44    D       │ 144   100   64    d
005   5     05    ENQ (enquiry)             │ 045   37    25    %     │ 105   69    45    E       │ 145   101   65    e
006   6     06    ACK (acknowledge)         │ 046   38    26    &     │ 106   70    46    F       │ 146   102   66    f
007   7     07    BEL '\a' (bell)           │ 047   39    27    '     │ 107   71    47    G       │ 147   103   67    g
010   8     08    BS  '\b' (backspace)      │ 050   40    28    (     │ 110   72    48    H       │ 150   104   68    h
011   9     09    HT  '\t' (horizontal tab) │ 051   41    29    )     │ 111   73    49    I       │ 151   105   69    i
012   10    0A    LF  '\n' (new line)       │ 052   42    2A    *     │ 112   74    4A    J       │ 152   106   6A    j
013   11    0B    VT  '\v' (vertical tab)   │ 053   43    2B    +     │ 113   75    4B    K       │ 153   107   6B    k
014   12    0C    FF  '\f' (form feed)      │ 054   44    2C    ,     │ 114   76    4C    L       │ 154   108   6C    l
015   13    0D    CR  '\r' (carriage ret)   │ 055   45    2D    -     │ 115   77    4D    M       │ 155   109   6D    m
016   14    0E    SO  (shift out)           │ 056   46    2E    .     │ 116   78    4E    N       │ 156   110   6E    n
017   15    0F    SI  (shift in)            │ 057   47    2F    /     │ 117   79    4F    O       │ 157   111   6F    o
020   16    10    DLE (data link escape)    │ 060   48    30    0     │ 120   80    50    P       │ 160   112   70    p
021   17    11    DC1 (device control 1)    │ 061   49    31    1     │ 121   81    51    Q       │ 161   113   71    q
022   18    12    DC2 (device control 2)    │ 062   50    32    2     │ 122   82    52    R       │ 162   114   72    r
023   19    13    DC3 (device control 3)    │ 063   51    33    3     │ 123   83    53    S       │ 163   115   73    s
024   20    14    DC4 (device control 4)    │ 064   52    34    4     │ 124   84    54    T       │ 164   116   74    t
025   21    15    NAK (negative ack.)       │ 065   53    35    5     │ 125   85    55    U       │ 165   117   75    u
026   22    16    SYN (synchronous idle)    │ 066   54    36    6     │ 126   86    56    V       │ 166   118   76    v
027   23    17    ETB (end of trans. blk)   │ 067   55    37    7     │ 127   87    57    W       │ 167   119   77    w
030   24    18    CAN (cancel)              │ 070   56    38    8     │ 130   88    58    X       │ 170   120   78    x
031   25    19    EM  (end of medium)       │ 071   57    39    9     │ 131   89    59    Y       │ 171   121   79    y
032   26    1A    SUB (substitute)          │ 072   58    3A    :     │ 132   90    5A    Z       │ 172   122   7A    z
033   27    1B    ESC (escape)              │ 073   59    3B    ;     │ 133   91    5B    [       │ 173   123   7B    {
034   28    1C    FS  (file separator)      │ 074   60    3C    <     │ 134   92    5C    \  '\\' │ 174   124   7C    |
035   29    1D    GS  (group separator)     │ 075   61    3D    =     │ 135   93    5D    ]       │ 175   125   7D    }
036   30    1E    RS  (record separator)    │ 076   62    3E    >     │ 136   94    5E    ^       │ 176   126   7E    ~
037   31    1F    US  (unit separator)      │ 077   63    3F    ?     │ 137   95    5F    _       │ 177   127   7F    DEL
```

A **character** is a figure that represents a concept or idea.  It seems a little obvious, but in computer science, we represent things by what they are.  For instance, all of the characters in the ASCII table for a **character set** also called an **alphabet**.  You may have know what an alphabet was from when you were young as being the characters A through Z paired with a lowercase counter part.  But the ASCII table represents an alphabet of 128 characters that have **subsets** of other characters. `0` through `9` for the numbers,  `A` through `Z` for those uppercase letters, `a` through `z` for the lowercase letters.  There's other subsets for visual and non-visual characters.  A **string** is a sequence of characters.  A **sequence** is a set of items (characters, strings, etc.) that are consecutively together.

#### Escape Sequences

JavaScript has ten escape sequences.  And **escape sequence** is a pair of characters that when used together expresses either a non-visual character or a literal character.  A **literal character** is a character that is what it is.  They won't be processed to have some alternate meaning like some escape sequences represent.

| Seq. | Produces        | Notes |
|------|-----------------|-------|
| `\0` | NULL Character  | This character represents nothing...literally nothing. The bits are all zeros in this character. In some languages like C and C++, a string is terminated with this character at the end. Do not follow this sequence with any digit character. |
| `[\b]` | Backspace       | Moves the cursor back one position. (This needs to be in square brackets otherwise it will be treated as a word boundary. We'll talk about those in the [Assertions](#Assertions) section later.) |
| `\t` | Horizontal tab  | Typically equivalent to four spaces. |
| `\n` | New Line        | Also know as a line feed, go to the next line. In Linux and Unix, this includes a carriage return. On Windows, a carriage return and new line characters are separate. |
| `\v` | Vertical tab    | Rarely used, it was once equivalent to six lines. |
| `\f` | Form Feed       | A form feed was once the page break control character. |
| `\r` | Carriage return | Back when there were typewriters, a carriage was the roller device that held the paper. The carriage return would return the carriage back to the position of the first character. |
| `\"` | Double quote    | If you are using a string that starts and ends with a double quote, you must escape any double quotes in between so that they are not interpreted as string delimiters.  Strings that use double quotes as delimiters, it will treat the above escape sequences as escape sequences rather than literal sequences. You don't need to escape the single quote, but you will still need to escape the backslash. |
| `\'` | Single quote    | If you are using a string that starts and ends with the single quote, you must escape any single quotes in between so that they are not interpreted as string delimiters. Strings that use single quotes as delimiters will treat all escape character sequences (except the one for single quote) as literal sequences. |
| `\\` | Backslash       | The literal backslash character. |

Regular expressions might require the escape of other characters.

| Seq. | Produces        | Notes |
|------|-----------------|-------|
| `\/` | (Forward) Slash | The forward slash only needs to be escaped if it is used in a regular expression. In some languages, to avoid having patterns that are in "slash hell", a different delimiter other than the slash is used |
| `\\|` | Pipe            | The pipe only needs to be escaped in regular expressions so that it isn't treated as the *alternate operator*. We'll explain later what this is later. |

There are other characters tha might need to be escaped, but we'll mention them later.

#### Character Ranges

Earlier, I had mentioned that it was important to understand the ASCII table because its alphabet was made of specific subsets.  These characters are sequentially together for a reason, because of their bits.  For instance, the hexadecimal columns represent the the "upper" and "lower" bits.

All of the number characters (`0` through `9`) have an upper bit value of `3` or `0011` in binary. So all the numbers have a sequence of `0011xxxx` where `x` is a value of `0` or `1`.

Uppercase letters (`A` through `Z`) have an upper bit sequence that start with `4` or `5`. That's `0100` or `0101`.  We can represent that with the first three bits with `010` such that each of the uppercase characters start with `010xxxxx`.

Lowercase letters (`a` through `Z`) have an upper bit sequence that start with `6` or `7`. That's `0110` or `0111`. We can represent that with the first three bits with `011` such that each of the lowercase characters start with `011xxxxx`.

Now, I know what you're  thinking "what about all those special characters"?  That is worth noting, yes, but look at where all the numbers and the letters are positioned.

In regular expression, we can represent ranges of characters by using **square brackets** (`[ ]`). These create **character classes** (a.k.a. character ranges).  For instance, to represent all of the lowercase vowels, we would define a character set of `[aeiou]` The regex will only match those characters witin the square brackets.  The set of uppercase vowels would be `[AEIOU]`.  For all vowels, `[AEIOUaeiou]` would be used.

OK, how about ALL the decimal number characters? We would use `[0123456789]`...but that is getting pretty long. Fortunately, we can use another character inside of the square brackets. The **hyphen** (a.k.a. dash) can be used to define a range of characters without needing to explicity use them all. They just need to be sequential on the ASCII table. So our `[0123456789]` becomes `[0-9]`.  If we need to match the hyphen, the hyphen should be the first characters in our range. (e.g. `[-0-9]`).

OK, so what if you don't want to match any number characters? You can use the **carat** as the first character in the range. `[^0-9]`.  If you need to match the carat, don't use it as the first character. (e.g. `[0-9^]`).

To represent *any* character, you can use the **period** (a.k.a. full stop or dot). For instance `c.t` will match `cat` or `cut` basically any single character.  To match the dot, you need to put the dot inside square brackets (`[.]`). We'll describe in further detail what the dot covers.

#### Special Character Classes

There are some escape sequences that we can use to represent some sequences.

| Seq.     | Equivalent | Definition | Notes |
|----------|------------|------------|-------|
| `.`      | `[^\n\r\u2028\u2029]` | Matches **any single character** *except* line terminals. To match the dot, you need to enclose it in square brackets. To have the dot match line terminals, you need to use the `s` ("dotAll") flag. (Note: Using the `m` (multiline) flag will not change the dot's behavior. If you need to match a pattern across multiple lines, use the carat character class (`[^]`) which will match any character including newlines.) |
| `\d`     | `[0-9]`         | Matches **digits**. |
| `\D`     | `[^0-9]`        | Do not match digits. |
| `\w`     | `[A-Za-z0-9_]`  | Matches **word** characters. |
| `\W`     | `[^A-Za-z0-9_]` | Do not match word characters. |
| `\s`     | `[\f\n\r\t\v\u0020\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]`  | Matches **whitespace** characters  |
| `\S`     | `[^\f\n\r\t\v\u0020\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]` | Do not match whitespace characters |
| `\cX`    | | Match a **control character** from `A` to `Z`. (e.g. `/\cM\cJ/` matches `\r\n`) |
| `\xhh`   | | Match a character represented by two **hexadecimal** digits. These characters are listed in the ASCII table. `h` represents a hexadecimal digit, `[0-9A-Fa-f]` |
| `\xhhhh` | | Match a UTF-16 code unit represented by four hexadecimal digits. |
| `\u{hhhh}` or `\u{hhhhh}` | | Matches the character with the Unicode value of `U+hhhh` or `U+hhhhh`. This is only enabled if the `u` flag is set. |
| `\p{UnicodeProperty}`   | | Matches any character based on its **Unicode character properties**. We'll talk about that later in [Unicode properties](#unicode-properties) |
| `\P{UnicodeProperty}`   | | Do not match a character based on its Unicode Property. |
| `x\|y`    | | The **disjunction** (alternate) operator matches either the pattern on the left or right to the pipe. This operator is sometimes called the **OR operator**.|

> Note: A disjunction is another way to specify a set of choices, but it is not a character class.  Disjunctions are not atoms. You need to use a **group** tok make it part of a bigger pattern.
> Functionally, `[abc]` is equivalent to `(?:a|b|c)`.

### Quantifiers

**Quantifiers** (also known as **closures** or **occurrence operators**) indicate *how many* times to match a set of characters or expressions.

Many of these quantifiers are single characters. In the case of `*` and `+` there are named after Stephen Kleene (1909-1904), who is regarded as the inventor of regular expression.


| Seq. | Equivalent | Meaning | Escape | Notes |
|------|------------|---------|--------|-------|
| `x*` | `x{0,}`    | The **Kleene star** or **star** means that `x` can occur zero or more times. | `\*` | |
| `x+` | `x{1,}`    | The **Kleene plus** or **plus** means that `x` can occur one or more times. | `\+` | |
| `x?` | `x{0,1}` | The **question mark** means that `x` can may not or may occur, that is `x` can occur zero or one time. | `\?` | If used immediately after `*`, `+`, `?`, or `{}`, this will make the quantifier "lazy". I'll describe this below.|
| `x{n}` | | `x` occurs exactly `n` times. | `\{` ... `\}` | `n` is a positive integer and `n` greater than zero. |
| `x{n,}` | | `x` occurs at least `n` times. | | `n` an integer is greater than or equal to zero. Take note of the comma!
| `x{n,m}` | | `x` occurs at least `n` times and at most `m` times. | | `n` an integer is greater than or equal to zero, `m` is a positive integer greater than `n`.

> NOTE: Remember not to escape any special character class characters next to unescaped curly braces as they could trigger their special meaning.

#### Lazy Quantifiers

By default, quantifiers are *greedy*, meaning that they try to match as much of the string as possible.  Adding a question mark (`?`) after a quantifier makes the quantifier *lazy* or *not-greedy*, meaning it will stop as soon as it finds a match.

| Greedy    | Lazy      |
|-----------|-----------|
| `x*`      | `x*?`     |
| `x+`      | `x+?`     |
| `x?`      | `x??`     |
| `x{n}`    | `x{n}?`   |
| `x{n,}`   | `x{n,}?`  |
| `x{n,m}`  | `x{n,m}?` |

#### Examples of Quantifiers in Action

Let's match three words using various patterns. We will show the part that appears when it is matched. Assume that no flags have been applied.

| Pattern  | `robot` | `boo` | `burns` |
|----------|---------|-------|---------|
| `/bo/`   | `bo`    | `bo`  |         |
| `/bo*/`  | `bo`    | `boo` | `b`     |
| `/bo+/`  | `bo`    | `boo` |         |
| `/bo?/`  | `bo`    | `bo`  | `b`     |
| `/bo*?/` | `b`     | `b`   | `b`     |
| `/bo+?/` | `bo`    | `bo`  |         |
| `/bo??/` | `b`     | `b`   | `b`     |

### Groups and Backreferences

**Groups** allow you to combine multiple patterns as whole.  There are actually three types of groups.

- **Capturing groups** which match a pattern and remembers what it matched by assigning matches to array starting at the index of `1` and up to `n`.
- **Named capturing groups** which match a pattern and can be assigned a name.
- **Non-capturing groups** which match a pattern but does not remember it.

**Backreferences** refer back to a previously captured group in the same expression.  Typically, backreferences are more useful if are using a program like `sed`, `awk`, or `vim` or scripting languages like Bash, Perl, or Ruby where you can literally use regular expressions to find and replace text.

> Note: `RegExp` objects have a set of predefined properties `$1` through `$9` where you can match up to the first nine instances using `$n` where `n` is a positive integer between `1` and `9`.  The same can be said for backreferences where `\n` and calls back the first nine instances.  The advantage of using the array format means you are not limited to matching the first nine instances.  The second advantage to array format is that the zeroth entry of the match array is the entire string, not just the parts defined by the capture group.

#### Groups

| Sequence | Meaning | Notes |
|----------|---------|-------|
| `(x)`    | **Capture group**: Match `x` and store it at `$1` or `[1]`. If we saved our result to `matches`, the full string that was matched would be at `matches[0]` and our subgroup with just `x` in it would be at `matches[1]`. | Capturing groups do have a performance penalty. Capture groups are not available in `String.prototype.match` if the global flag is used at the end of a regular expression. |
| `(<Name>x)` | **Named capture group**: Matches `x` and stores it on the `.group` property of the returned matches under the name specified by `<Name>`. If we saved our results to `matches`, we would recall the named group as `matches.groups.Name` | Angle brackets are required for a group name. |
| `(?:x)` | **Non-capturing group**: Matches `x` but does not store it. Basically, this just groups a pattern to keep it together.  | Using this with the altenative operator means that `(?:a\|b\|c)` is equivalent to `[abc]`.  Because there is nothing to store, there's not as much of a loss in performance.

#### Backreferences

| Sequence | Meaning | Notes |
|----------|---------|-------|
| `\n` | **Numbered backreference** Backreference to a previous substring match starting from the left-most match of `\1`. | `n` is a positive integer.
| `\k<Name>` | **Named backreference**: Backreference to a previous substring match specified by `<Name>`. | `\k` is used literally to indicate the beginning of a backreference to a named capture group. |

#### Examples of Groups and Backreferences

> TODO: Fill in this part later.

### Assertions

**Assertions** are used to indicate boundaries and other patterns where a match should be.

There are three types of assertions

- Boundary-type Assertions
- Look Assertions
- Conditional Assertions

**Assertions** include boundaries, which indicate the beginning and ending of words. This is why we have that rare `[\b]` for backspace and `\b` for word boundaries in JavaScript.  It also indicates other patterns where a match is possible including look-ahead, look-behind, and conditional expressions.

If you need to use the literal characters in these assertions, I've include a column for how to write them if you need to escape them.

#### Boundary-type Assertions

**Boundary-type assertions** (also called **anchors**) are assertions that indicate teh beginning and ending of words. This is why we have that rare `[\b]` for backspace and `\b` for word boundaries in JavaScript.

| Seq. | Meaning | Escape | Notes |
|------|---------|--------|-------|
| `^`  | Match at the begining of input. | `\^` | Remember that if a carat is used at the beginning of a character class, it negates matching the characters in that class. |
| `$`  | Match at the end of input. | `\$` | |
| `\b` | Match a **word boundary**. | `[\b]` | When escaped in a character class, `\b` means "match the backspace" |
| `\B` | Do not match a word boundary | | |

Typically `^` and `$` can be used in the same expression to exactly match what is on a line. If there are no characters in between, such that the regular expression is `/^$/`, it will match any empty lines. This can be useful if you want to remove any lines that are empty.

If the multiline flag is set to true, the carat will also match immediately after a line break character and the dollar sign will also match immediately before a line break character.

The `\b` and `\B` assertions are used to match (or not match) word boundaries.  A **word boundary** is the position where a word character (defined by `\w` or `[A-Za-z0-9_]`) is not followed or preceded by another word character, such as between a letter and a space.

A matched word boundary is not included in the match.  In other words, `\b` and `\B` have a length of zero.

##### Examples of Boundary-type Assertions

- `/^A/` will match a line that starts with something like `Alexander Hamilton` but it will not match `alexander hamilton` unless the case-insensitive flag is used.
- `/r$/` will match a line than ends with `Aaron Burr` but will not match `sir.` because there is a period at the end.
- `/\bm/` will match the `m` in `moon`.
- `/oo\b/` will not match the `oo` in `moon` because of the `n` at the end.
- `/oon\b/` will match the `oon` in `moon` because a word character can never be followed by both a non-word and a word character.
- `/\Bon/` will match `on` or `moon` or `at noon`.  `/ye\B/` will match `possibly yesterday`.

#### Look Assertions

A **look assertion** is the most complex assertion. Look assertions use *capturing groups*, which are defined by parenthesis when matching, and question mark next to the opening parenthesis.  If you want to use the literal characters in these assertions, you will need to escape the parenthesis and the question mark so they are not processed using their special meaning.

There are four types of assertions

| Sequence  | Assertion           | Meaning | Notes |
|-----------|---------------------|---------|-------|
| `x(?=y)`  | Lookahead           | Match `x` only if `x` is followed by `y`     | |
| `x(?!y)`  | Negative Lookahead  | Match `x` only if `x` is not followed by `y` | |
| `(?<=y)x` | Lookbehind          | Match `x` only if `x` is preceded by `y`     | |
| `(?<!y)x` | Negative Lookbehind | Match `x` only if `x` is not preceded by `y` | |

Note that `x` and `y` can be more than one character long.  You can also use the *alternative operator* (`|`) to match more than one item inside the parenthesis.

It should be noted that `y` is not part of the match results.

##### Examples of Look Assertions

- `/Jack(?=Sparrow)/` matches `Jack` only if it is followed by `Sparrow`.
- `/Jack(?=Sparrow|Frost)/` matches `Jack` only if it is followed by `Sparrow` or `Frost`.
- `/\d+(?!\.)/` matches any number only if it is NOT followed by a decimal point.
  - `/\d+(?!\.)/.exec('3.14159')` matches `14159` but not `3`.
- `/(?<=Eleanor)Roosevelt/` matches `Roosevelt` only if it is preceded by `Eleanor`.
- `/(?<=Franklin|Theodore)Roosevelt/` matches `Roosevelt` only if it is preceded by `Franklin` or `Theodore`.
- `/(?<!-)\d+/` matches only a number if it is NOT preceded by a minus sign.
  - `/(?<!-)\d+/.exec('3')` matches `3`.
  - `/(?<!-)\d+/.exec('-3')` does NOT match anything because of the minus before the `3`.

#### Conditional Assertions

You can use groups with look assertions to create **conditional assertions**.

```javascript
const rainbow = "red orange yellow green blue indigo violet";
const re_la  = /(?:(?=green)blue|indigo)/
const re_nla = /(?:(?!green)blue|indigo)/
console.log(rainbow.match(re_la));    // Array ["indigo"]
console.log(rainbow.match(re_nla));   // Array ["blue"]

const re_la2  = /(?:(?=blue)blue|indigo)/
const re_nla2 = /(?:(?!blue)blue|indigo)/
console.log(rainbow.match(re_la2));   // Array ["blue"]
console.log(rainbow.match(re_nla2));  // Array ["indigo"]

const re_la3  = /(?:(?=blue)green|indigo)/
const re_nla3 = /(?:(?!blue)green|indigo)/
console.log(rainbow.match(re_la3));   // Array ["indigo"]
console.log(rainbow.match(re_nla3));  // Array ["green"]
```

> TODO: Try to explain this better. I've spent like an hour trying to understand why it does everything backwards. Maybe it was [the source that I learned it from](https://www.regular-expressions.info/conditional.html).

### Unicode Property Escapes

**Unicode Property Escapes** (UPEs) are regular expressions that allow for the matching of characters based on their Unicode properties. A character is described by several properties which are either binary ("boolean-like") or non-binary.  For instance, UPEs can be used to match emojis, punctuations, and letter from specific languages or scripts.

In order for them to work, you will need to use the `u` flag to indicate that a string must be considered as a series of Unicode points.  When enabled, this may extend some character classes (such as `\w` which match only latin letters `a` through `z`) to include more characters for better support.  Probably letters with accent marks.

#### UPE Syntax

```text
// Non-binary values
\p{UnicodePropertyValue}
\p{UnicodePropertyName=UnicodePropertyValue}

// Binary and non-binary values
\p{UnicodeBinaryPropertyName}

// Negation: \P is negated \p
\P{UnicodePropertyValue}
\P{UnicodeBinaryPropertyName}
```

Where

- `UnicodeBinaryPropertyName` is the name of a binary property. See [this list](https://www.unicode.org/Public/UCD/latest/ucd/PropList.txt)
- `UnicodePropertyName` is the name of a non-binary property. This includes
  - `General_Category` (`gc`) See [this list](https://www.unicode.org/Public/UCD/latest/ucd/UnicodeData.txt)
  - `Script` (`sc`) See [this list](https://www.unicode.org/Public/UCD/latest/ucd/Scripts.txt)
  - `Script_Extensions` (`scx`)
- `UnicodePropertyValue` is a token that may be an alias, shorthand, or longhand value. See [this list](https://www.unicode.org/Public/UCD/latest/ucd/PropertyValueAliases.txt)

#### A Short List of Unicode Properties

Unicode is pretty flexible with how you can use it, so in JavaScript, if you use a UPE, you should be able to use `\p{Lu}`, `\p{lu}`, `\p{uppercase letter}`, `\p{Uppercase Letter}`, `\p{Uppercase_Letter}`, or `\p{uppercaseletter}` and it should produce the same results.

> Note: This doesn't include everything. Just the ones you might use or could see.

This table shows the `General_Category` property values.

| Alias | Long Form |
|-------|-----------|
| `L`  | **Letter** |
| `Lu` | Uppercase Letter |
| `Ll` | Lowercase Letter |
| `Lt` | Titlecase Letter |
| `Lm` | Modified Letter |
| `Lo` | Other Letter |
| `M`  | **Mark** |
| `Mn` | Non-spacing Mark
| `Mc` | Spacing Combining Mark |
| `Me` | Enclosing Mark |
| `N`  | **Number** |
| `Nd` | Decimal Digit Number |
| `Nl` | Letter Number |
| `No` | Other Number |
| `S`  | **Symbol** |
| `Sm` | Math Symbol |
| `Sc` | Currency Symbol |
| `Sk` | Modifier Symbol |
| `So` | Other Symbol |
| `P`  | **Punctuation** |
| `Pc` | Connector Punctuation |
| `Pd` | Dash Punctuation |
| `Ps` | Open Punctuation |
| `Pe` | Close Punctuation |
| `Pi` | Initial Punctuation |
| `Pf` | Final Punctuation |
| `Po` | Other Punctuation |
| `Z`  | **Separator** |
| `Zs` | Space Separator |
| `Zl` | Line Separator |
| `Zp` | Paragraph Separator |
| `C`  | **Other** |
| `Cc` | Control |
| `Cf` | Format |
| `Cs` | Surrogate |
| `Co` | Private Use |
| `Cn` | Unassigned |

The `General_Category` does not include these values

| Value    | Equivalent           | Matches | Reason for Exclusion |
|----------|----------------------|---------|----------------------|
| Any      | `[\u{0}-\u{10FFFF}]` | All Code Points | In some regular expression languages, `\p{Any}` may be expressed with a period (`.`), but that may exclude new line characters. |
| Assigned | `\P{Cn}`             | All assigned characters (for the target version of Unicode) | This also includes all private use characters. It is useful for avoiding confusing double complements. Note that `Cn` includes noncharacters, so *Assigned* excludes them. |
| ASCII    | `[\u{0}-\u{7F}]`     | All ASCII Characters | This is basically a subset of various sets described. |

## JavaScript RegExp properties and methods

> Note: Deprecated methods are not included.

### `RegExp`

#### RegExp constructor

- `RegExp(pattern, [flags])` : Create a new `RegExp` object

#### RegExp static properties

- `get RegExp[@@species]` : The constructor function that is used to created derived objects

#### RegExp instance properties

- `RegExp.prototype.constructor` : literal constructor for a regular expression.
- `RegExp.prototype.flags` : Indicate what flags are used. (See [Flags](#Flags))
- `RegExp.prototype.dotAll` : Whether `.` matches newlines or not.
- `RegExp.prototype.global` : Whether to match all instance or just the first instance
- `RegExp.prototype.hasIndices` : Whether or not to enable `indices` when using the `RegExp.prototype.exec()` method.
- `RegExp.prototype.ignoreCase` : Whether to ignore case for letters.
- `RegExp.prototype.multiline` : Wheter to match across lines.
- `RegExp.prototype.source` : Return the literal regular expression as a string
- `RegExp.prototype.sticky` : Whether to track the index of matches
- `RegExp.prototype.unicode` : Whether to enable `\u` and `\p` to match Unicode characters

- `lastIndex` : The index at which to start the next match. This property belongs to each `RegExp` instance. (It's not documented very well.)

#### RegExp instance methods

- `RegExp.prototype.exec(string)` : Execute a search for a match in a specifc string and returns a result array or `null`.  Use `lastIndex` to get the most recent starting position.
- `RegExp.prototype.test(string)` : Test if a string matches a regular expression. Returns a boolean.
- `RegExp.prototype.toString()` : Returns a string representation of the regex pattern.
- `RegExp.prototype[@@match]()` : Specifies how `String.prototype.match()` should work.
- `RegExp.prototype[@@matchAll]()` : Specifies how `String.prototype.matchAll()` should work.
- `RegExp.prototype[@@replace]()` : Specifies how `String.prototype.replace()` and `String.prototype.replaceAll()` should work.
- `RegExp.prototype[@@search]()` : Specifies how `String.prototype.search()` should work.
- `RegExp.prototype[@@split]()`  : Specifies how `String.prototype.split()` should work.

### `String`

#### String instance methods

- `String.prototype.match(pattern :RegExp)` : Find the first match of a mattern. Should return an `Array` or `null`.
- `String.prototype.matchAll(pattern :RegExp)` : Find all matches of a pattern, return as an `Array` or `null`.
- `String.prototype.replace(pattern :StringOrRegExp,replacement :StringOrFunction)` : Find and replace the first instance of a matching `String` with the another `String` or the output of a  `Function` processed by the match found.
- `String.prototype.replaceAll(pattern :StringOrRegExp,replacement :StringOrFunction)` : Like the previous function, but for all instances.
- `String.prototype.search(pattern :RegExp)` : Return the position of where a `RegExp` matches a `String`. Should return `-1` if nothing is found.
- `String.prototype.split(separator :StringOrRegExp, [limit:Number])` : Use a `String` or a `RegExp` to split an `String` into a `Array`.  Include a `limit` to indicate how many times this should happen otherwise it will split everywhere the `separator` is found.

---
## About the Author

Thank you for reading my regular expression tutorial.

Visit my [website](https://www.jrcharney.com/), [github profile](https://github.com/jrcharney/), or [Bootcamp portofolio](https://jrcharney.github.io/bootcamp-portfolio/).

## Sources

> TODO: Cite your sources! And format it.

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String
