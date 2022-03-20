


# Internal Representation

There are three core data types defined recursively

```ocaml
(*Ocaml*)
type constant = Text of String
				| Number of Int
				| Number of float
				| Blank of Int
				| Nothing
				| Lowerlevel of statement
and statement = Statement of constant list * constant list
```

## Constate Type

### Basic Constructor 

The constant type represent the atomic types of the markup language. The `Text` constructor takes in a string and the `Number` constructor and take in either an integer or a float.

### Blank Constructor 

The `Blank` constructor represents the amount of additional white space inline. It takes in an integer as an input to represent the corresponding amount of inline white space. For example, `Blank(5)` means $5 mm$  of inline whitespace.

During parsing, the `Blank` constructor will be used if the lexer encounter more than one  white space between any two words in the input string or encounters more than one `linebreak` symbols (`\n`). When the white space is found, the lexer will count the number f whitespace in terms of the space character and input the count to the `Blank` constructor.

Example 1: When there is only one white space between any two words.

markup language:
```
\{}{Something and something else}
```
OCaml:
```ocaml
Statement([],[Text("Something"), Text("and"), Text("something"), Text("else")])
```
Latex:
```latex
\begin{}
	Something and something else
\end{}
```

Example 2: When there is more than one white space between words.

Markup Language
```
\{}{something     and  something else}
```
OCaml:
```ocaml
Statement([],[Text("something"), Blank(5), Text("and"), Blank(2), Text("something"), Text("else")])
```
Latex:
```latex
\begin{}
	something \hsapce{5mm} and \hsapce{2mm} something else
\end{}
```

When the lexer encounters a `tab` symbol (`\t`) in the input string, the tab will be represented as  `Blank(6)`

Example 3: There is a tab in the input string

Markup Language:
```
\{}{something		and something   else}
```
OCaml:
```ocaml
Statement([],[Text("something"), Blank(6), Text("and"), Text("something"), Blank(3), Text("else")])
```
Latex:
```latex
\begin{}
	something \hsapce{6mm} and something \hspace{3mm} else
\end{}
```

### Nothing constructor

The `Nothing` constructor exists to allow sections of a `statement` to be empty. And in further state of the development when variables are introduced to the grammar, to allow a variable name to point to an empty value.

### Lowerlevel constructor

The `Lowerlevel` constructor takes a `statement` as an input. Its purpose is to allow the `content` of a statement to contain another statement in case additional style is needed to be applied to selected section of the statement. A statement that is contained in the `content` section of another statement is called a *lower level (child) statement* and will automatically inherit all the style of its parent unless explicitly overwritten in the style section of the child statement.

## Statement Type

As indicated in the OCaml code at the top, the `Statement` constructor take a tuple of two element as its input. The first element is a list of constant, which represent the style that will be applied (explained in later section). The second element is another list of constant, which represent the content that will be styled.

Example: (style sheet is in the next section)

Markup Language:

```
\{mt-6 mb-6 ml-2 mr-2}{something and \{text-xl text-bold}{something   else} and \{text-2xl text-ital}{someother things} 1234}
```

OCaml

```ocaml
Statement([Text("mt-6"), Text("mb-6"), Text("ml-3"), Text("mr-2")], 
			[Text("something"), Text("and"), 
			 Lowerlevel(Statement([Text("text-xl"), Text("Text-bold")],
								  [Text("something"), Blank(3),Text("else")]
								)
						),
			 Text("and"),
			 Lowerlevel(Statement([Text("text-2xl"), Text("text-ital")],
								 [Text("someother"), Text("things")]
								 )
						),
			Number(1234)
			]
		)
```

Latex

```latex
\vspace{6mm}
	\begin{addmargin}[3em]{2em}
		something and
		\begin{textbf}
		\begin{large}
			something \hsapce{3mm} else
		\end{large}
		\end{textbf}
		and
		\begin{emph}
		\begin{Large}
			someother things
		\begin{Large}
		\end{emph}
	\end{addmargin}
\vspace{6mm}
```

# Style sheet

## White Space

| markup language | Latex | meaning |
|--|--|--|
| mt-$n$ | \vspace{$n$ mm} | margin top: $n$ mm where $n$ is user input number |
| mb-$n$ | \vspace{1mm} | margin bottom: $n$ mm where $n$ is user input number |
| ml-$n$ | \begin{addmargin}[1em]{0em} | margin left: $n$ mm where $n$ is user input number ||
| mr-$n$ | \begin{addmargin}[0em]{3em} | margin right: $n$ mm where $n$ is user input number |

## Font Size

| markup language | Latex | Description |
|--|--|--|
| text-s | \small | font size small |
| text-xs | \footnotesize | font size smaller |
| text-xxs | \tiny | the smallest font accessible through keyword  |
| text-l | \large | large font size |
| text-xl | \Large | larger font size |
| text-xxl | \LARGE | The largest font accessible through keyword |
| text-$n$-$m$| \fontsize{$n$ pt}{$m$ pt}| custom font size where $n$ and $m$ are user input|
