# パーサージェネレータ　PEG.jsを使おう。

## パーサージェネレータって何？

構文を解析するものを作る、プログラミング言語
作るための基本

## 簡単な例：ワードカウンタを作る。

wordCounterのパーサーは語の繰り返しからなるとしよう。

```
wordCounter = word*

```

wordとは文字の繰り返しからなるとしよう。

```
word = letter+

```




## 文法

There are several types of parsing expressions, some of them containing subexpressions and thus forming a recursive structure:


|文法|内容|
|:--|:--|
|"literal"|リテラル、特定の文字列|
|[characters]|文字列セット [a-z]で小文字のセット、^で逆転|
|( expression )|()内で表現された式に合致するかどうかを調べます |
|expression *|式に0回以上マッチするかを調べます |
|expression +|式に1回以上マッチするかを調べます|
|expression ?|式にマッチするかを挑戦し、マッチすれば、その式の内容、マッチしなければnullを返却します。|






"literal"
'literal'

    Match exact literal string and return it. The string syntax is the same as in JavaScript. Appending i right after the literal makes the match case-insensitive.
.

    Match exactly one character and return it as a string.
[characters]

    Match one character from a set and return it as a string. The characters in the list can be escaped in exactly the same way as in JavaScript string. The list of characters can also contain ranges (e.g. [a-z] means “all lowercase letters”). Preceding the characters with ^ inverts the matched set (e.g. [^a-z] means “all character but lowercase letters”). Appending i right after the literal makes the match case-insensitive.
rule

    Match a parsing expression of a rule recursively and return its match result.
( expression )

    Match a subexpression and return its match result.
expression *

    Match zero or more repetitions of the expression and return their match results in an array. The matching is greedy, i.e. the parser tries to match the expression as many times as possible. Unlike in regular expressions, there is no backtracking.
expression +

    Match one or more repetitions of the expression and return their match results in an array. The matching is greedy, i.e. the parser tries to match the expression as many times as possible. Unlike in regular expressions, there is no backtracking.
expression ?

    Try to match the expression. If the match succeeds, return its match result, otherwise return null. Unlike in regular expressions, there is no backtracking.
    
& expression

    Try to match the expression. If the match succeeds, just return undefined and do not consume any input, otherwise consider the match failed.
! expression

    Try to match the expression. If the match does not succeed, just return undefined and do not consume any input, otherwise consider the match failed.
& { predicate }

    The predicate is a piece of JavaScript code that is executed as if it was inside a function. It gets the match results of labeled expressions in preceding expression as its arguments. It should return some JavaScript value using the return statement. If the returned value evaluates to true in boolean context, just return undefined and do not consume any input; otherwise consider the match failed.

    The code inside the predicate can access all variables and functions defined in the initializer at the beginning of the grammar.

    The code inside the predicate can also access location information using the location function. It returns an object like this:

    {
      start: { offset: 23, line: 5, column: 6 },
      end:   { offset: 23, line: 5, column: 6 }
    }

    The start and end properties both refer to the current parse position. The offset property contains an offset as a zero-based index and line and column properties contain a line and a column as one-based indices.

    The code inside the predicate can also access options passed to the parser using the options variable.

    Note that curly braces in the predicate code must be balanced.
! { predicate }

    The predicate is a piece of JavaScript code that is executed as if it was inside a function. It gets the match results of labeled expressions in preceding expression as its arguments. It should return some JavaScript value using the return statement. If the returned value evaluates to false in boolean context, just return undefined and do not consume any input; otherwise consider the match failed.

    The code inside the predicate can access all variables and functions defined in the initializer at the beginning of the grammar.

    The code inside the predicate can also access location information using the location function. It returns an object like this:

    {
      start: { offset: 23, line: 5, column: 6 },
      end:   { offset: 23, line: 5, column: 6 }
    }

    The start and end properties both refer to the current parse position. The offset property contains an offset as a zero-based index and line and column properties contain a line and a column as one-based indices.

    The code inside the predicate can also access options passed to the parser using the options variable.

    Note that curly braces in the predicate code must be balanced.
$ expression

    Try to match the expression. If the match succeeds, return the matched text instead of the match result.
label : expression

    Match the expression and remember its match result under given label. The label must be a JavaScript identifier.

    Labeled expressions are useful together with actions, where saved match results can be accessed by action's JavaScript code.
expression1 expression2 ... expressionn

    Match a sequence of expressions and return their match results in an array.
expression { action }

    Match the expression. If the match is successful, run the action, otherwise consider the match failed.

    The action is a piece of JavaScript code that is executed as if it was inside a function. It gets the match results of labeled expressions in preceding expression as its arguments. The action should return some JavaScript value using the return statement. This value is considered match result of the preceding expression.

    To indicate an error, the code inside the action can invoke the expected function, which makes the parser throw an exception. The function takes two parameters — a description of what was expected at the current position and optional location information (the default is what location would return — see below). The description will be used as part of a message of the thrown exception.

    The code inside an action can also invoke the error function, which also makes the parser throw an exception. The function takes two parameters — an error message and optional location information (the default is what location would return — see below). The message will be used by the thrown exception.

    The code inside the action can access all variables and functions defined in the initializer at the beginning of the grammar. Curly braces in the action code must be balanced.

    The code inside the action can also access the text matched by the expression using the text function.

    The code inside the action can also access location information using the location function. It returns an object like this:

    {
      start: { offset: 23, line: 5, column: 6 },
      end:   { offset: 25, line: 5, column: 8 }
    }

    The start property refers to the position at the beginning of the expression, the end property refers to position after the end of the expression. The offset property contains an offset as a zero-based index and line and column properties contain a line and a column as one-based indices.

    The code inside the action can also access options passed to the parser using the options variable.

    Note that curly braces in the action code must be balanced.
expression1 / expression2 / ... / expressionn

    Try to match the first expression, if it does not succeed, try the second one, etc. Return the match result of the first successfully matched e