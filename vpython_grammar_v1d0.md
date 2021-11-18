  <style type="text/css">
    ::-moz-selection
    {
      color: #FFFFFF;
      background: #141414;
    }
    ::selection
    {
      color: #FFFFFF;
      background: #141414;
    }
    .ebnf a, .grammar a
    {
      text-decoration: none;
    }
    .ebnf a:hover, .grammar a:hover
    {
      color: #0F0F0F;
      text-decoration: underline;
    }
    .signature
    {
      color: #4D4D4D;
      font-size: 11px;
      text-align: right;
    }
    body
    {
      font: normal 12px Verdana, sans-serif;
      color: #141414;
      background: #FFFFFF;
    }
    a:link, a:visited
    {
      color: #141414;
    }
    a:link.signature, a:visited.signature
    {
      color: #4D4D4D;
    }
    a.button, #tabs li a
    {
      padding: 0.25em 0.5em;
      border: 1px solid #4D4D4D;
      background: #E3E3E3;
      color: #4D4D4D;
      text-decoration: none;
      font-weight: bold;
    }
    a.button:hover, #tabs li a:hover
    {
      color: #0F0F0F;
      background: #F0F0F0;
      border-color: #0F0F0F;
    }
    #tabs
    {
      padding: 3px 10px;
      margin-left: 0;
      margin-top: 58px;
      border-bottom: 1px solid #141414;
    }
    #tabs li
    {
      list-style: none;
      margin-left: 5px;
      display: inline;
    }
    #tabs li a
    {
      border-bottom: 1px solid #141414;
    }
    #tabs li a.active
    {
      color: #141414;
      background: #FFFFFF;
      border-color: #141414;
      border-bottom: 1px solid #FFFFFF;
      outline: none;
    }
    #divs div
    {
      display: none;
      overflow:auto;
    }
    #divs div.active
    {
      display: block;
    }
    #text
    {
      border-color: #4D4D4D;
      background: #FFFFFF;
      color: #0F0F0F;
    }
    .small
    {
      vertical-align: top;
      text-align: right;
      font-size: 9px;
      font-weight: normal;
      line-height: 120%;
    }
    td.small
    {
      padding-top: 0px;
    }
    .hidden
    {
      visibility: hidden;
    }
    td:hover .hidden
    {
      visibility: visible;
    }
    div.download
    {
      display: none;
      background: #FFFFFF;
      position: absolute;
      right: 34px;
      top: 94px;
      padding: 10px;
      border: 1px dotted #141414;
    }
    #divs div.ebnf, .ebnf code
    {
      display: block;
      padding: 10px;
      background: #F0F0F0;
      width: 992px;
    }
    #divs div.grammar
    {
      display: block;
      padding-left: 16px;
      padding-top: 2px;
      padding-bottom: 2px;
      background: #F0F0F0;
    }
    pre
    {
      margin: 0px;
    }
    .ebnf div
    {
      padding-left: 13ch;
      text-indent: -13ch;
    }
    .ebnf code, .grammar code, textarea, pre
    {
      font:12px SFMono-Regular,Consolas,Liberation Mono,Menlo,Courier,monospace;
    }
    tr.option-line td:first-child
    {
      text-align: right
    }
    tr.option-text td
    {
      padding-bottom: 10px
    }
    table.palette
    {
      border-top: 1px solid #0F0F0F;
      border-right: 1px solid #0F0F0F;
      margin-bottom: 4px
    }
    td.palette
    {
      border-bottom: 1px solid #0F0F0F;
      border-left: 1px solid #0F0F0F;
    }
    a.palette
    {
      padding: 2px 3px 2px 10px;
      text-decoration: none;
    }
    .palette
    {
      -webkit-user-select: none;
      -khtml-user-select: none;
      -moz-user-select: none;
      -o-user-select: none;
      -ms-user-select: none;
    }
  </style>

program:

![](images/vpython/program.png)

`

[program](#program "program")  ::= [lines](#lines "lines"){.code}

`

no references

  

lines:

![](images/vpython/lines.png)

`

[lines](#lines "lines")    ::= [line](#line "line")+{.ebnf.code}

`

referenced by:

*   [codeblock](#codeblock "codeblock")
*   [program](#program "program")

  

line:

![](images/vpython/line.png)

`

[line](#line "line")     ::= [statements](#statements "statements") '↵'?

           | '↵'

`

referenced by:

*   [lines](#lines "lines")

  

statements:

![](images/vpython/statements.png)

`

[statements](#statements "statements")

         ::= [statement](#statement "statement")+

`

referenced by:

*   [line](#line "line")
*   [statement](#statement "statement")

  

statement:

![](images/vpython/statement.png)

`

[statement](#statement "statement")

         ::= ( ( 'for' [ident](#ident "ident") 'in' | 'while' | 'elif' ) [expression](#expression "expression") | [fndefinition](#fndefinition "fndefinition") '(' ( [ident](#ident "ident") ( '=' [expression](#expression "expression") )? )? ( ',' [ident](#ident "ident") ( '=' [expression](#expression "expression") )? )* ')' ) ':' [codeblock](#codeblock "codeblock")

           | 'if' [expression](#expression "expression") ':' ( [codeblock](#codeblock "codeblock") ( 'elif' [expression](#expression "expression") ':' [codeblock](#codeblock "codeblock") )* ( 'else' ':' [codeblock](#codeblock "codeblock") )? | [statements](#statements "statements") )

           | ( [identscalararraylhs](#identscalararraylhs "identscalararraylhs") '=' | [identscalararray](#identscalararray "identscalararray") [opassgn](#opassgn "opassgn") | 'print' ) [expression](#expression "expression")

           | ( ( 'exit' | 'quit' ) '(' | 'native'? [ident](#ident "ident") '(' [fncallargs](#fncallargs "fncallargs") | 'alias' '(' [ident](#ident "ident") ',' [expression](#expression "expression") ) ')'

           | 'return' [expression](#expression "expression")?

           | 'pass'

           | '@' [ident](#ident "ident")

`

referenced by:

*   [statements](#statements "statements")

  

arrayaccessor:

![](images/vpython/arrayaccessor.png)

`

[arrayaccessor](#arrayaccessor "arrayaccessor")

         ::= ( '[' [expression](#expression "expression") ']' )+

`

referenced by:

*   [identscalararray](#identscalararray "identscalararray")
*   [identscalararraylhs](#identscalararraylhs "identscalararraylhs")

  

fncallargs:

![](images/vpython/fncallargs.png)

`

[fncallargs](#fncallargs "fncallargs")

         ::= [expression](#expression "expression")? ( ',' [expression](#expression "expression") )*

`

referenced by:

*   [statement](#statement "statement")
*   [value](#value "value")

  

fndefinition:

![](images/vpython/fndefinition.png)

`

[fndefinition](#fndefinition "fndefinition")

         ::= 'def' [ident](#ident "ident")

`

referenced by:

*   [statement](#statement "statement")

  

codeblock:

![](images/vpython/codeblock.png)

`

[codeblock](#codeblock "codeblock")

         ::= '↵' '⇥' [lines](#lines "lines") '⇤'

`

referenced by:

*   [statement](#statement "statement")

  

opassgn:

![](images/vpython/opassgn.png)

`

[opassgn](#opassgn "opassgn")  ::= '+='

           | '-='

           | '*='

           | '/='

           | '%='

           | '**='

           | '//='

`

referenced by:

*   [statement](#statement "statement")

  

declareident:

![](images/vpython/declareident.png)

`

[declareident](#declareident "declareident")

         ::= [ident](#ident "ident")

`

no references

  

expression:

![](images/vpython/expression.png)

`

[expression](#expression "expression")

         ::= 'not'? [logical_and_expression](#logical_and_expression "logical_and_expression") ( 'or' [logical_and_expression](#logical_and_expression "logical_and_expression") )*

`

referenced by:

*   [arrayaccessor](#arrayaccessor "arrayaccessor")
*   [fncallargs](#fncallargs "fncallargs")
*   [multiplicative\_expression](#multiplicative_expression "multiplicative_expression")
*   [statement](#statement "statement")
*   [value](#value "value")

  

logical\_and\_expression:

![](images/vpython/logical_and_expression.png)

`

[logical_and_expression](#logical_and_expression "logical_and_expression")

         ::= [equality_expression](#equality_expression "equality_expression") ( 'and' [equality_expression](#equality_expression "equality_expression") )*

`

referenced by:

*   [expression](#expression "expression")

  

equality\_expression:

![](images/vpython/equality_expression.png)

`

[equality_expression](#equality_expression "equality_expression")

         ::= [relational_expression](#relational_expression "relational_expression") ( ( '==' | [NEQ](#NEQ "NEQ") | 'is' ) [relational_expression](#relational_expression "relational_expression") )*

`

referenced by:

*   [logical\_and\_expression](#logical_and_expression "logical_and_expression")

  

relational\_expression:

![](images/vpython/relational_expression.png)

`

[relational_expression](#relational_expression "relational_expression")

         ::= [additive_expression](#additive_expression "additive_expression") ( ( '>' | '<' | '<=' | '>=' ) [additive_expression](#additive_expression "additive_expression") )*

`

referenced by:

*   [equality\_expression](#equality_expression "equality_expression")

  

additive\_expression:

![](images/vpython/additive_expression.png)

`

[additive_expression](#additive_expression "additive_expression")

         ::= [multiplicative_expression](#multiplicative_expression "multiplicative_expression") ( ( '+' | '-' ) [multiplicative_expression](#multiplicative_expression "multiplicative_expression") )*

`

referenced by:

*   [relational\_expression](#relational_expression "relational_expression")

  

multiplicative\_expression:

![](images/vpython/multiplicative_expression.png)

`

[multiplicative_expression](#multiplicative_expression "multiplicative_expression")

         ::= ( [value](#value "value") | ( 'str' '(' [expression](#expression "expression") | 'input' '(' [expression](#expression "expression")? ) ')' | '[' [expression](#expression "expression") ( ',' [expression](#expression "expression") )* ']' ( '*' [expression](#expression "expression") )? ) ( ( '*' | '/' | '//' | '%' | '**' ) [value](#value "value") )*

`

referenced by:

*   [additive\_expression](#additive_expression "additive_expression")

  

value:

![](images/vpython/value.png)

`

[value](#value "value")    ::= [constant](#constant "constant")

           | ( '(' [expression](#expression "expression") | 'native'? [ident](#ident "ident") '(' [fncallargs](#fncallargs "fncallargs") | ( 'id' | 'symbol' ) '(' [ident](#ident "ident") ) ')'

           | [identscalararray](#identscalararray "identscalararray")

`

referenced by:

*   [multiplicative\_expression](#multiplicative_expression "multiplicative_expression")

  

identscalararray:

![](images/vpython/identscalararray.png)

`

[identscalararray](#identscalararray "identscalararray")

         ::= [ident](#ident "ident") [arrayaccessor](#arrayaccessor "arrayaccessor")?

`

referenced by:

*   [statement](#statement "statement")
*   [value](#value "value")

  

identscalararraylhs:

![](images/vpython/identscalararraylhs.png)

`

[identscalararraylhs](#identscalararraylhs "identscalararraylhs")

         ::= [ident](#ident "ident") [arrayaccessor](#arrayaccessor "arrayaccessor")?

`

referenced by:

*   [statement](#statement "statement")

  

ident:

![](images/vpython/ident.png)

`

[ident](#ident "ident")    ::= [identifier](#identifier "identifier")

`

referenced by:

*   [declareident](#declareident "declareident")
*   [fndefinition](#fndefinition "fndefinition")
*   [identscalararray](#identscalararray "identscalararray")
*   [identscalararraylhs](#identscalararraylhs "identscalararraylhs")
*   [statement](#statement "statement")
*   [value](#value "value")

  

constant:

![](images/vpython/constant.png)

`

[constant](#constant "constant") ::= [unary_operator](#unary_operator "unary_operator")? ( 'integer' | 'real' )

           | [HEX](#HEX "HEX")

           | 'string'

           | 'True'

           | 'False'

           | 'None'

`

referenced by:

*   [value](#value "value")

  

unary\_operator:

![](images/vpython/unary_operator.png)

`

[unary_operator](#unary_operator "unary_operator")

         ::= '+'

           | '-'

`

referenced by:

*   [constant](#constant "constant")

  

identifier:

![](images/vpython/identifier.png)

`

[identifier](#identifier "identifier")

         ::= ( [letter](#letter "letter") | '_' ) ( [letter](#letter "letter") | [digit](#digit "digit") | '_' )*

`

referenced by:

*   [ident](#ident "ident")

  

NEQ:

![](images/vpython/NEQ.png)

`

[NEQ](#NEQ "NEQ")      ::= '<>'

           | '!='

`

referenced by:

*   [equality\_expression](#equality_expression "equality_expression")

  

letter:

![](images/vpython/letter.png)

`

[letter](#letter "letter")   ::= 'A'

           | 'B'

           | 'C'

           | 'D'

           | 'E'

           | 'F'

           | 'G'

           | 'H'

           | 'I'

           | 'J'

           | 'K'

           | 'L'

           | 'M'

           | 'N'

           | 'O'

           | 'P'

           | 'Q'

           | 'R'

           | 'S'

           | 'T'

           | 'U'

           | 'V'

           | 'W'

           | 'X'

           | 'Y'

           | 'Z'

           | 'a'

           | 'b'

           | 'c'

           | 'd'

           | 'e'

           | 'f'

           | 'g'

           | 'h'

           | 'i'

           | 'j'

           | 'k'

           | 'l'

           | 'm'

           | 'n'

           | 'o'

           | 'p'

           | 'q'

           | 'r'

           | 's'

           | 't'

           | 'u'

           | 'v'

           | 'w'

           | 'x'

           | 'y'

           | 'z'

`

referenced by:

*   [identifier](#identifier "identifier")

  

digit:

![](images/vpython/digit.png)

`

[digit](#digit "digit")    ::= '0'

           | '1'

           | '2'

           | '3'

           | '4'

           | '5'

           | '6'

           | '7'

           | '8'

           | '9'

`

referenced by:

*   [HEX](#HEX "HEX")
*   [identifier](#identifier "identifier")

  

HEX:

![](images/vpython/HEX.png)

`

[HEX](#HEX "HEX")      ::= '0x' ( [digit](#digit "digit") | 'A' | 'B' | 'C' | 'D' | 'E' | 'F' )+

`

referenced by:

*   [constant](#constant "constant")

  

* * *

 

... generated by [RR - Railroad Diagram Generator](https://bottlecaps.de/rr/ui "https://bottlecaps.de/rr/ui")

[![](images/vpython/rr-1.63.png)](https://bottlecaps.de/rr/ui "https://bottlecaps.de/rr/ui")