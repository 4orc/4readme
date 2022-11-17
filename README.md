```css
  
readme.lua: LUA to Markdown. Assumes a simple Hungarian notation.
(c) 2022 Tim Menzies <timm@ieee.org> BSD-2clause license

Usage: lua readme.lua  [-h] [file1.lua file2.lua ...] > doco.md

Options:
 -h --help Show help 
```
> Extract doco from LUA files to Markdown. Assumes a simple Hungarian notation.	
 	
For example, this file was generated via	
 	
      lua readme.lua readme.lua > README.md	
    	
## Why this code?	
I love documentation and I do not love most documentation 	
generators. 	
- Why are they so complex to use? 	
- Why can't they be really short and easy to change?	
- Why can't they just create tables for the functions,	
generated from in-line comments around the code? 	
- And if I use	
just a few simple naming conventions, why can't they add type	
hints to my favorite untyped languages (lua, lisp, etc)?	
  	
## Conventions	
 	
1. Lines with Markdown start with `-- ` (and  we will print those).	
2. We only show help on public function.	
3. Public functions are denoted with a  trailing "-->", followed by 	
   return type then some comment text. e.g.<br> 	
   `function fred(s) --> str; Returns `s`, written as a string`<br>   	
   Note the semi-colon. Do not skip it (its important).	
4. In public function arguments, lower case versions of class type 	
   (e.g. `data`) are instances of that type (e.g.  `data` are `DATA` 	
   so `datas` is a list of `DATA` instances).	
5  Built in types are num, str, tab, bool, fun	
6. User-defined types are ny word starting with two upper case 	
   leading letters is a class; e.g. DATA	
7. Public function arguments have the following type hints:	
   	
What        | Notes                                                                            	
:-----------|:--------------------------------------------	
2 blanks    | 2 blanks denote start of optional arguments 	
4 blanks    | 4 blanks denote start of local arguments   	
n           | prefix for numerics                       	
is          | prefix for booleans                   	
s           | prefix for strings                   	
suffix s    | list of thing (so `sfiles` is list of strings)	
suffix fun  | suffix for functions                                            	
  	
## Guessing types	

<dl>
<dt><b> are.of(s:`str`) &rArr;  ?str </b></dt><dd>   top level, guesses a variable's type </dd>
</dl>

Types are either singular (one thing) or plural (a set of	
things). The naming conventions for plurals is the same as	
singulars, we just add an `s`. E.g. `bools` is a table of	
booleans. and `ns` is a table of `n`umbers.	
Singulars are either `bools`, `fun` (function),	
`n` (number), `s` (string), or `t` (table).	

<dl>
<dt><b> are.bool(s:`str`) &rArr;  ?"bool" </b></dt><dd>  names starting with "is" are booleans </dd>
<dt><b> are.fun(s:`str`) &rArr;  ?"fun" </b></dt><dd>  names ending in "fun" are functions </dd>
<dt><b> are.num(s:`str`) &rArr;  ?"n" </b></dt><dd>  names start with "n" are numbers  </dd>
<dt><b> are.str(s:`str`) &rArr;  ?"s" </b></dt><dd>  names starting with "s" are strings </dd>
<dt><b> are.tbl(s:`str`) &rArr;  ?"tab" </b></dt><dd>  names ending the "s" are tables </dd>
</dl>

## Low-level utilities	

<dl>
<dt><b> hint(s1:`str`, type) &rArr;  str </b></dt><dd>  if we know a type, add to arg (else return arg) </dd>
<dt><b> pretty(s:`str`) &rArr;  str </b></dt><dd>  clean up the signature (no spaces, no local vars) </dd>
<dt><b> optional(s:`str`) &rArr;  str </b></dt><dd>  removes local vars, returns the rest as a string </dd>
<dt><b> lines(sFilename:`str`,  fun:`fun`) &rArr;  nil </b></dt><dd>  call `fun` on csv rows. </dd>
<dt><b> dump() &rArr;  nil </b></dt><dd>  if we have any tbl contents, print them then zap tbl </dd>
</dl>

## Main	

<dl>
<dt><b> main(sFiles:`{str}`) &rArr;  nil </b></dt><dd>  for all lines on command line, print doco to standard output </dd>
</dl>

