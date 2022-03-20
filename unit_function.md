# Unit Fucntion

***

##### is_lower_case

```ocaml
 let is_lower_case c = 'a' <= c && c <= 'z'
```

This function takes an `char` as an input. It return `true` if the character is lower case and `false` if the character is upper case. If the input is not of type `char` or if the character is not a member of the alphabet the interpreter will throw an type error.

##### is_upper_case

```ocaml
 let is_lower_case c = 'A' <= c && c <= 'Z'
```

This function takes an `char` as an input. It return `true` if the character is upper case and `false` if the character is lower case. If the input is not of type `char` or if the character is not a member of the alphabet the interpreter will throw an type error.


##### is_alpha

```ocaml
 let is_alpha c = is_lower_case c || is_upper_case c
```

This function takes an `char` as an input. It returns `true` if the character is an element of the alphabet and `false` if the input is not. If the input is not of type `char`, the interpreter will throw an type error.

##### is_digit

```ocaml
let is_digit c = 
    '0' <= c && c <= '9'
```
This function takes an `char` as an input. It returns `true` if the character is an element of the numberic digits and `false` if the input is not. If the input is not of type `char`, the interpreter will throw an type error.

##### is_alphanum

```ocaml
let is_alphanum c = 
    is_alpha c ||
    is_digit
```

This function takes an `char` as an input. It returns `true` if the input an element of the alphabet or if the character is an element of the numberic digits and `false` if the input is not. If the input is not of type `char`, the interpreter will throw an type error.

##### is_blank

```ocaml
String.contains "\012\n\r\t" c
```

This function takes in an `char` as an input. It returns `true` if the input is an line break character and `flase` otherwise. If the input the not of type `char`, the interpreter will throw an type error.

##### explode

```ocaml
let explode s = List.of_seq(String.to_seq s)
```

This function takes an `String` as an input and return a output of `char list` that is a list of characters that is contained in the `String`.

##### implode

```ocaml
let implode ls = String.of_seq (List.to_seq ls)
```

This function takes an `char list` as an input and return an `String` that is constructed by appending each of the element in the `char list` to an empty list in order.

##### readlines

```ocaml
let readlines (file : string) : string =
  let fp = open_in file in
  let rec loop () =
    match input_line fp with
    | s -> s ^ "\n" ^ (loop ())
    | exception End_of_file -> ""
  in
  let res = loop () in
  let () = close_in fp in
  res
```

This function takes in an `String` as input. It use that the string to find a file in the local directory with the corresponding name. If that file was found, the function then read in the content in that line line bu line.