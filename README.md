pa\_extjs
=========

This is a fork of the `pa_js` syntax extension of `js_of_ocaml`. The official
source tree is [here](http://ocsigen.org/darcsweb/?r=js_of_ocaml;a=summary).
The original file is [here](http://ocsigen.org/darcsweb/?r=js_of_ocaml;a=headblob;f=/lib/syntax/pa_js.ml).

This version contains a custom syntax extension to express Javascript object
literals, that is used by
[ocaml-extjs](https://github.com/astrada/ocaml-extjs/). 

### Example

```ocaml
class type js_class = object
  method p1 : Js.js_string Js.t Js.prop
  method p2 : bool Js.t Js.prop
end

let obj : js_class Js.t = {|
  p1 = Js.string "string property";
  p2 = Js._true;
|}
```

is expanded to:

```ocaml
let obj : js_class Js.t =
  Js.Unsafe.obj [|
    ("p1", Js.Unsafe.inject (Js.string "string property"));
    ("p2", Js.Unsafe.inject Js._true);
  |]
```

and properties `p1` and `p2` are also type checked with respect to their
arguments. So that, for example:

```ocaml
let obj : js_class Js.t = {|
  p1 = Js._true;
  p2 = Js.string "string property";
|}
```

won't compile.

### Acknowledgement

This feature was inspired by Jacques Garrigue's [OO syntax
extension](http://www.math.nagoya-u.ac.jp/~garrigue/code/ocaml.html).

