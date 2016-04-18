##Lua词法
Lua使用的一遍扫描代码文件的同时生成opcode指令，这么做主要是为了解释执行的速度。但是速度快的背后，带来的却是这一部分代码比较难以理解。可以说，由于我编译部分基础知识的匮乏，这一部分是给我带来最大障碍的代码。

Lua的遍历使用的是递归下降分析，其EBNF词法如下，基本上是按照lparser.c中的函数对照来整理的，读者可以不必在前期完全理解这些内容，后面会按照不同的指令来进行分析，到时候需要对照着这部分EBNF进行理解：

```

chunk -> { stat [`;'] }

stat -> ifstat | whilestat | dostat | forstat | repeatstat | funcstat | localstat | retstat | breakstat | exprstat

ifstat -> IF cond THEN block {ELSEIF cond THEN block} [ELSE block] END

whilestat -> WHILE cond DO block END

dostat -> DO block END

forstat -> FOR {fornum | forlist} END

repeatstat -> REPEAT block UNTIL cond

funcstat -> FUNCTION funcname body

localstat -> LOCAL function Name funcbody | LOCAL NAME {`,' NAME} [`=' explist1]

retstat -> RETURN [explist1]

breakstat -> BREAK

exprstat -> primaryexp

block ->

cond ->

fornum ->

forlist ->

funcname ->

body ->

primaryexp -> prefixexp {`.' NAME | `[' exp `]' | `:' NAME funcargs | funcargs }

prefixexp -> NAME | `(' exp `)'

funcargs -> `(' explist1 `)' | constructor | STRING

exp -> subexpr

subexpr -> (simpleexp | unop subexpr) {binop subexpr}

simpleexp -> NUMBER | STRING | NIL | true | false | ... | constructor | FUNCTION body | primaryexp

explist1 -> expr {`,' expr}

constructor -> `{' [ fieldlist ]  `}'

fieldlist -> field { fieldsep field } [ fieldsep ]

field -> ‘[’ exp ‘]’ ‘=’ exp | name ‘=’ exp | exp

fieldsep -> ‘,’ | ‘;’

fieldlist ->

unop ->

binop ->

```


