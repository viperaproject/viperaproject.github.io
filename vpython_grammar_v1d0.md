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
 
      <p style="font-size: 14px; font-weight:bold"><a name="program">program:</a></p><img border="0" src="images/vpython/program.png" height="37" width="109" usemap="#program.map"><map name="program.map"><area shape="rect" coords="29,1,79,33" href="#lines" title="lines"></map><p>
         
         <div class="ebnf">
             <code>
               <div><a href="#program" title="program">program</a>&nbsp;&nbsp;::= <a href="#lines" title="lines">lines</a></div>
               </code>
            </div>
         </p>
      
      <p>no references</p><br><p style="font-size: 14px; font-weight:bold"><a name="lines">lines:</a></p><img border="0" src="images/vpython/lines.png" height="53" width="141" usemap="#lines.map"><map name="lines.map"><area shape="rect" coords="49,17,91,49" href="#line" title="line"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#lines" title="lines">lines</a>&nbsp;&nbsp;&nbsp;&nbsp;::= <a href="#line" title="line">line</a>+</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#codeblock" title="codeblock">codeblock</a></li>
            
            <li><a href="#program" title="program">program</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="line">line:</a></p><img border="0" src="images/vpython/line.png" height="113" width="277" usemap="#line.map"><map name="line.map"><area shape="rect" coords="49,1,141,33" href="#statements" title="statements"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#line" title="line">line</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= <a href="#statements" title="statements">statements</a> '↵'?</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '↵'</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#lines" title="lines">lines</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="statements">statements:</a></p><img border="0" src="images/vpython/statements.png" height="53" width="185" usemap="#statements.map"><map name="statements.map"><area shape="rect" coords="49,17,135,49" href="#statement" title="statement"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#statements" title="statements">statements</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= <a href="#statement" title="statement">statement</a>+</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#line" title="line">line</a></li>
            
            <li><a href="#statement" title="statement">statement</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="statement">statement:</a></p><img border="0" src="images/vpython/statement.png" height="923" width="1175" usemap="#statement.map"><map name="statement.map"><area shape="rect" coords="147,1,199,33" href="#ident" title="ident"><area shape="rect" coords="291,1,381,33" href="#expression" title="expression"><area shape="rect" coords="69,155,159,187" href="#fndefinition" title="fndefinition"><area shape="rect" coords="245,187,297,219" href="#ident" title="ident"><area shape="rect" coords="387,219,477,251" href="#expression" title="expression"><area shape="rect" coords="621,155,673,187" href="#ident" title="ident"><area shape="rect" coords="763,187,853,219" href="#expression" title="expression"><area shape="rect" coords="1043,1,1125,33" href="#codeblock" title="codeblock"><area shape="rect" coords="97,307,187,339" href="#expression" title="expression"><area shape="rect" coords="291,307,373,339" href="#codeblock" title="codeblock"><area shape="rect" coords="335,263,425,295" href="#expression" title="expression"><area shape="rect" coords="655,339,737,371" href="#codeblock" title="codeblock"><area shape="rect" coords="271,383,363,415" href="#statements" title="statements"><area shape="rect" coords="69,427,211,459" href="#identscalararraylhs" title="identscalararraylhs"><area shape="rect" coords="69,471,191,503" href="#identscalararray" title="identscalararray"><area shape="rect" coords="211,471,285,503" href="#opassgn" title="opassgn"><area shape="rect" coords="325,427,415,459" href="#expression" title="expression"><area shape="rect" coords="191,647,243,679" href="#ident" title="ident"><area shape="rect" coords="309,647,389,679" href="#fncallargs" title="fncallargs"><area shape="rect" coords="185,723,237,755" href="#ident" title="ident"><area shape="rect" coords="301,723,391,755" href="#expression" title="expression"><area shape="rect" coords="151,799,241,831" href="#expression" title="expression"><area shape="rect" coords="101,887,153,919" href="#ident" title="ident"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#statement" title="statement">statement</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= ( ( 'for' <a href="#ident" title="ident">ident</a> 'in' | 'while' | 'elif' ) <a href="#expression" title="expression">expression</a> | <a href="#fndefinition" title="fndefinition">fndefinition</a> '(' ( <a href="#ident" title="ident">ident</a> ( '=' <a href="#expression" title="expression">expression</a> )? )? ( ',' <a href="#ident" title="ident">ident</a> ( '=' <a href="#expression" title="expression">expression</a> )? )* ')' ) ':' <a href="#codeblock" title="codeblock">codeblock</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'if' <a href="#expression" title="expression">expression</a> ':' ( <a href="#codeblock" title="codeblock">codeblock</a> ( 'elif' <a href="#expression" title="expression">expression</a> ':' <a href="#codeblock" title="codeblock">codeblock</a> )* ( 'else' ':' <a href="#codeblock" title="codeblock">codeblock</a> )? | <a href="#statements" title="statements">statements</a> )</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| ( <a href="#identscalararraylhs" title="identscalararraylhs">identscalararraylhs</a> '=' | <a href="#identscalararray" title="identscalararray">identscalararray</a> <a href="#opassgn" title="opassgn">opassgn</a> | 'print' ) <a href="#expression" title="expression">expression</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| ( ( 'exit' | 'quit' ) '(' | 'native'? <a href="#ident" title="ident">ident</a> '(' <a href="#fncallargs" title="fncallargs">fncallargs</a> | 'alias' '(' <a href="#ident" title="ident">ident</a> ',' <a href="#expression" title="expression">expression</a> ) ')'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'return' <a href="#expression" title="expression">expression</a>?</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'pass'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '@' <a href="#ident" title="ident">ident</a></div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#statements" title="statements">statements</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="arrayaccessor">arrayaccessor:</a></p><img border="0" src="images/vpython/arrayaccessor.png" height="53" width="281" usemap="#arrayaccessor.map"><map name="arrayaccessor.map"><area shape="rect" coords="95,17,185,49" href="#expression" title="expression"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#arrayaccessor" title="arrayaccessor">arrayaccessor</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= ( '[' <a href="#expression" title="expression">expression</a> ']' )+</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#identscalararray" title="identscalararray">identscalararray</a></li>
            
            <li><a href="#identscalararraylhs" title="identscalararraylhs">identscalararraylhs</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="fncallargs">fncallargs:</a></p><img border="0" src="images/vpython/fncallargs.png" height="85" width="423" usemap="#fncallargs.map"><map name="fncallargs.map"><area shape="rect" coords="49,49,139,81" href="#expression" title="expression"><area shape="rect" coords="263,17,353,49" href="#expression" title="expression"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#fncallargs" title="fncallargs">fncallargs</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= <a href="#expression" title="expression">expression</a>? ( ',' <a href="#expression" title="expression">expression</a> )*</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#statement" title="statement">statement</a></li>
            
            <li><a href="#value" title="value">value</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="fndefinition">fndefinition:</a></p><img border="0" src="images/vpython/fndefinition.png" height="37" width="173" usemap="#fndefinition.map"><map name="fndefinition.map"><area shape="rect" coords="91,1,143,33" href="#ident" title="ident"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#fndefinition" title="fndefinition">fndefinition</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= 'def' <a href="#ident" title="ident">ident</a></div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#statement" title="statement">statement</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="codeblock">codeblock:</a></p><img border="0" src="images/vpython/codeblock.png" height="37" width="255" usemap="#codeblock.map"><map name="codeblock.map"><area shape="rect" coords="125,1,175,33" href="#lines" title="lines"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#codeblock" title="codeblock">codeblock</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= '↵' '⇥' <a href="#lines" title="lines">lines</a> '⇤'</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#statement" title="statement">statement</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="opassgn">opassgn:</a></p><img border="0" src="images/vpython/opassgn.png" height="301" width="147"><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#opassgn" title="opassgn">opassgn</a>&nbsp;&nbsp;::= '+='</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '-='</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '*='</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '/='</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '%='</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '**='</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '//='</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#statement" title="statement">statement</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="declareident">declareident:</a></p><img border="0" src="images/vpython/declareident.png" height="37" width="111" usemap="#declareident.map"><map name="declareident.map"><area shape="rect" coords="29,1,81,33" href="#ident" title="ident"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#declareident" title="declareident">declareident</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= <a href="#ident" title="ident">ident</a></div></code></div>
         </p>
      
      <p>no references</p><br><p style="font-size: 14px; font-weight:bold"><a name="expression">expression:</a></p><img border="0" src="images/vpython/expression.png" height="113" width="371" usemap="#expression.map"><map name="expression.map"><area shape="rect" coords="151,45,321,77" href="#logical_and_expression" title="logical_and_expression"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#expression" title="expression">expression</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= 'not'? <a href="#logical_and_expression" title="logical_and_expression">logical_and_expression</a> ( 'or' <a href="#logical_and_expression" title="logical_and_expression">logical_and_expression</a> )*</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#arrayaccessor" title="arrayaccessor">arrayaccessor</a></li>
            
            <li><a href="#fncallargs" title="fncallargs">fncallargs</a></li>
            
            <li><a href="#multiplicative_expression" title="multiplicative_expression">multiplicative_expression</a></li>
            
            <li><a href="#statement" title="statement">statement</a></li>
            
            <li><a href="#value" title="value">value</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="logical_and_expression">logical_and_expression:</a></p><img border="0" src="images/vpython/logical_and_expression.png" height="81" width="247" usemap="#logical_and_expression.map"><map name="logical_and_expression.map"><area shape="rect" coords="49,45,197,77" href="#equality_expression" title="equality_expression"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#logical_and_expression" title="logical_and_expression">logical_and_expression</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= <a href="#equality_expression" title="equality_expression">equality_expression</a> ( 'and' <a href="#equality_expression" title="equality_expression">equality_expression</a> )*</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#expression" title="expression">expression</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="equality_expression">equality_expression:</a></p><img border="0" src="images/vpython/equality_expression.png" height="169" width="257" usemap="#equality_expression.map"><map name="equality_expression.map"><area shape="rect" coords="49,133,207,165" href="#relational_expression" title="relational_expression"><area shape="rect" coords="49,45,95,77" href="#NEQ" title="NEQ"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#equality_expression" title="equality_expression">equality_expression</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= <a href="#relational_expression" title="relational_expression">relational_expression</a> ( ( '==' | <a href="#NEQ" title="NEQ">NEQ</a> | 'is' ) <a href="#relational_expression" title="relational_expression">relational_expression</a> )*</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#logical_and_expression" title="logical_and_expression">logical_and_expression</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="relational_expression">relational_expression:</a></p><img border="0" src="images/vpython/relational_expression.png" height="213" width="247" usemap="#relational_expression.map"><map name="relational_expression.map"><area shape="rect" coords="49,177,197,209" href="#additive_expression" title="additive_expression"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#relational_expression" title="relational_expression">relational_expression</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= <a href="#additive_expression" title="additive_expression">additive_expression</a> ( ( '&gt;' | '&lt;' | '&lt;=' | '&gt;=' ) <a href="#additive_expression" title="additive_expression">additive_expression</a> )*</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#equality_expression" title="equality_expression">equality_expression</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="additive_expression">additive_expression:</a></p><img border="0" src="images/vpython/additive_expression.png" height="125" width="279" usemap="#additive_expression.map"><map name="additive_expression.map"><area shape="rect" coords="49,89,229,121" href="#multiplicative_expression" title="multiplicative_expression"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#additive_expression" title="additive_expression">additive_expression</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= <a href="#multiplicative_expression" title="multiplicative_expression">multiplicative_expression</a> ( ( '+' | '-' ) <a href="#multiplicative_expression" title="multiplicative_expression">multiplicative_expression</a> )*</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#relational_expression" title="relational_expression">relational_expression</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="multiplicative_expression">multiplicative_expression:</a></p><img border="0" src="images/vpython/multiplicative_expression.png" height="293" width="771" usemap="#multiplicative_expression.map"><map name="multiplicative_expression.map"><area shape="rect" coords="49,17,103,49" href="#value" title="value"><area shape="rect" coords="173,61,263,93" href="#expression" title="expression"><area shape="rect" coords="209,137,299,169" href="#expression" title="expression"><area shape="rect" coords="115,225,205,257" href="#expression" title="expression"><area shape="rect" coords="359,257,449,289" href="#expression" title="expression"><area shape="rect" coords="647,17,701,49" href="#value" title="value"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#multiplicative_expression" title="multiplicative_expression">multiplicative_expression</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= ( <a href="#value" title="value">value</a> | ( 'str' '(' <a href="#expression" title="expression">expression</a> | 'input' '(' <a href="#expression" title="expression">expression</a>? ) ')' | '[' <a href="#expression" title="expression">expression</a> ( ',' <a href="#expression" title="expression">expression</a> )* ']' ( '*' <a href="#expression" title="expression">expression</a> )? ) ( ( '*' | '/' | '//' | '%' | '**' ) <a href="#value" title="value">value</a> )*</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#additive_expression" title="additive_expression">additive_expression</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="value">value:</a></p><img border="0" src="images/vpython/value.png" height="289" width="505" usemap="#value.map"><map name="value.map"><area shape="rect" coords="49,1,123,33" href="#constant" title="constant"><area shape="rect" coords="115,45,205,77" href="#expression" title="expression"><area shape="rect" coords="191,89,243,121" href="#ident" title="ident"><area shape="rect" coords="309,89,389,121" href="#fncallargs" title="fncallargs"><area shape="rect" coords="243,165,295,197" href="#ident" title="ident"><area shape="rect" coords="49,253,171,285" href="#identscalararray" title="identscalararray"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#value" title="value">value</a>&nbsp;&nbsp;&nbsp;&nbsp;::= <a href="#constant" title="constant">constant</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| ( '(' <a href="#expression" title="expression">expression</a> | 'native'? <a href="#ident" title="ident">ident</a> '(' <a href="#fncallargs" title="fncallargs">fncallargs</a> | ( 'id' | 'symbol' ) '(' <a href="#ident" title="ident">ident</a> ) ')'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| <a href="#identscalararray" title="identscalararray">identscalararray</a></div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#multiplicative_expression" title="multiplicative_expression">multiplicative_expression</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="identscalararray">identscalararray:</a></p><img border="0" src="images/vpython/identscalararray.png" height="69" width="279" usemap="#identscalararray.map"><map name="identscalararray.map"><area shape="rect" coords="29,1,81,33" href="#ident" title="ident"><area shape="rect" coords="121,33,229,65" href="#arrayaccessor" title="arrayaccessor"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#identscalararray" title="identscalararray">identscalararray</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= <a href="#ident" title="ident">ident</a> <a href="#arrayaccessor" title="arrayaccessor">arrayaccessor</a>?</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#statement" title="statement">statement</a></li>
            
            <li><a href="#value" title="value">value</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="identscalararraylhs">identscalararraylhs:</a></p><img border="0" src="images/vpython/identscalararraylhs.png" height="69" width="279" usemap="#identscalararraylhs.map"><map name="identscalararraylhs.map"><area shape="rect" coords="29,1,81,33" href="#ident" title="ident"><area shape="rect" coords="121,33,229,65" href="#arrayaccessor" title="arrayaccessor"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#identscalararraylhs" title="identscalararraylhs">identscalararraylhs</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= <a href="#ident" title="ident">ident</a> <a href="#arrayaccessor" title="arrayaccessor">arrayaccessor</a>?</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#statement" title="statement">statement</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="ident">ident:</a></p><img border="0" src="images/vpython/ident.png" height="37" width="135" usemap="#ident.map"><map name="ident.map"><area shape="rect" coords="29,1,105,33" href="#identifier" title="identifier"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#ident" title="ident">ident</a>&nbsp;&nbsp;&nbsp;&nbsp;::= <a href="#identifier" title="identifier">identifier</a></div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#declareident" title="declareident">declareident</a></li>
            
            <li><a href="#fndefinition" title="fndefinition">fndefinition</a></li>
            
            <li><a href="#identscalararray" title="identscalararray">identscalararray</a></li>
            
            <li><a href="#identscalararraylhs" title="identscalararraylhs">identscalararraylhs</a></li>
            
            <li><a href="#statement" title="statement">statement</a></li>
            
            <li><a href="#value" title="value">value</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="constant">constant:</a></p><img border="0" src="images/vpython/constant.png" height="301" width="385" usemap="#constant.map"><map name="constant.map"><area shape="rect" coords="69,33,187,65" href="#unary_operator" title="unary_operator"><area shape="rect" coords="49,89,93,121" href="#HEX" title="HEX"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#constant" title="constant">constant</a>&nbsp;::= <a href="#unary_operator" title="unary_operator">unary_operator</a>? ( 'integer' | 'real' )</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| <a href="#HEX" title="HEX">HEX</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'string'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'True'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'False'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'None'</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#value" title="value">value</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="unary_operator">unary_operator:</a></p><img border="0" src="images/vpython/unary_operator.png" height="81" width="129"><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#unary_operator" title="unary_operator">unary_operator</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= '+'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '-'</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#constant" title="constant">constant</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="identifier">identifier:</a></p><img border="0" src="images/vpython/identifier.png" height="203" width="267" usemap="#identifier.map"><map name="identifier.map"><area shape="rect" coords="49,123,103,155" href="#letter" title="letter"><area shape="rect" coords="163,89,217,121" href="#letter" title="letter"><area shape="rect" coords="163,45,211,77" href="#digit" title="digit"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#identifier" title="identifier">identifier</a></div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= ( <a href="#letter" title="letter">letter</a> | '_' ) ( <a href="#letter" title="letter">letter</a> | <a href="#digit" title="digit">digit</a> | '_' )*</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#ident" title="ident">ident</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="NEQ">NEQ:</a></p><img border="0" src="images/vpython/NEQ.png" height="81" width="139"><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#NEQ" title="NEQ">NEQ</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= '&lt;&gt;'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '!='</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#equality_expression" title="equality_expression">equality_expression</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="letter">letter:</a></p><img border="0" src="images/vpython/letter.png" height="2281" width="133"><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#letter" title="letter">letter</a>&nbsp;&nbsp;&nbsp;::= 'A'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'B'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'C'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'D'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'E'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'F'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'G'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'H'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'I'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'J'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'K'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'L'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'M'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'N'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'O'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'P'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'Q'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'R'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'S'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'T'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'U'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'V'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'W'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'X'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'Y'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'Z'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'a'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'b'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'c'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'd'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'e'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'f'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'g'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'h'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'i'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'j'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'k'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'l'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'm'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'n'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'o'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'p'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'q'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'r'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 's'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 't'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'u'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'v'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'w'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'x'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'y'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| 'z'</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#identifier" title="identifier">identifier</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="digit">digit:</a></p><img border="0" src="images/vpython/digit.png" height="433" width="127"><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#digit" title="digit">digit</a>&nbsp;&nbsp;&nbsp;&nbsp;::= '0'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '1'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '2'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '3'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '4'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '5'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '6'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '7'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '8'</div>
               
               <div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| '9'</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#HEX" title="HEX">HEX</a></li>
            
            <li><a href="#identifier" title="identifier">identifier</a></li>
            </ul>
         </p><br><p style="font-size: 14px; font-weight:bold"><a name="HEX">HEX:</a></p><img border="0" src="images/vpython/HEX.png" height="317" width="243" usemap="#HEX.map"><map name="HEX.map"><area shape="rect" coords="125,17,173,49" href="#digit" title="digit"></map><p>
         
         <div class="ebnf"><code>
               
               <div><a href="#HEX" title="HEX">HEX</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;::= '0x' ( <a href="#digit" title="digit">digit</a> | 'A' | 'B' | 'C' | 'D' | 'E' | 'F' )+</div></code></div>
         </p>
      
      <p>referenced by:
         
         <ul>
            
            <li><a href="#constant" title="constant">constant</a></li>
            </ul>
         </p><br><hr>
      
      <p>
         
         <table class="signature" border="0">
            
            <tr>
               
               <td style="width: 100%">&nbsp;</td>
               
               <td valign="top">
                  
                  <nobr class="signature">... generated by <a name="Railroad-Diagram-Generator" class="signature" title="https://bottlecaps.de/rr/ui" href="https://bottlecaps.de/rr/ui" target="_blank">RR - Railroad Diagram Generator</a></nobr>
                  </td>
               
               <td><a name="Railroad-Diagram-Generator" title="https://bottlecaps.de/rr/ui" href="https://bottlecaps.de/rr/ui" target="_blank"><img border="0" src="images/vpython/rr-1.63.png" height="16" width="16"></a></td>
               </tr>
            </table>
         </p>