bit3 coding standards
=====

These are our coding standards.

This coding standard based on the great TYPO3 coding standard, see
http://forge.typo3.org/projects/team-php_codesniffer/wiki/Using_the_TYPO3_Coding_Standard

Philosophy
-----

Our philosophy: Write fast readable and comprehensible code.
Don't be shy to write more lines, if it makes each line more atomic.

**What is the benefit of atomic lines?**
When each line is full atomic (only contains one operation), the order of lines reflect the execution order of the commands.

When you have a complex line like this:
```php
$variable = ($foo != $bar && count($zap) || $dig) ? $this->func($zap, $foo, $bar) : $this->other(count($zap))->chain($foo, $bar);
```
How is the order of execution? What do this code? You can understand if you analyse the line, but its hard to do.
Simplified this is the execution order of the operations:
```
evaluate $foo != $bar
execute count($zap)
evaluate count($zap)
evaluate .. && ..
evaluate $zip
evaluate .. || ..
if true
  execute $this->func($zap, $foo, $bar)
else
  execute count($zap)
  evaluate count($zap)
  execute $this->other(count($zap))
  execute $..->chain($foo, $bar)
assign $variable
```

If you want to understand what this code do, you have to understand the execution order of the operations and the meaning of each operation.
If you write this in a more atomic style, you make it easier to understand, for others and yourself if you nod touch the code a long time!

Compare against this, it is exactly the same code:
```php
if (
	$foo != $bar && count($zap) ||
	$dig
) {
	$var = $this->func($zap, $foo, $bar);
}
else {
	$var = $this->other(count($zap))->chain($foo, $bar);
}
```
As you can see, this snippet reflect the execution order and it is easier to understand.

Usage
=====

```bash
$ git clone git@github.com:bit3/php-coding-standard.git
$ phpcs --standard=/path/to/php-coding-standard/Bit3/ruleset.xml /path/to/my/source
```

Whitespaces
=====

Do **not** use spaces for indention. Use TAB.

Add spaces to split operators:
* assignment: `$var = 'value'`
* parameters: `func($a, $b, $c)`
* arrays: `array('foo' => 'bar', 'bar' => 'foo')`
* comments: `// comment` or `* multiline comment`

Class and function declaration
=====

Place opening brace `{` on new line for all non-control-structures like class or function declaration.
Place `extends` and `implements` on new line and indent one time.
```php
class Foo
	extends Bar
	implements Zap
{
	public function func()
	{
		...
	}
}
```

```php
function func()
{
	...
}
```

Don't add a space before opening bracked `(` in function declarations or calls.

Control structures
=====

Place opening brace '{' on same line for all control-structures.
Add newline after closing brace '}', even the control-structure continues.
```php
if (...) {
	...
}
else if (...) {
	...
}
else {
	...
}
```

```php
for (...) {
	...
}
```

```php
foreach (...) {
	...
}
```

```php
while (...) {
	...
}
```

```php
do {
	...
}
while (...);
```

If control-structure condition gets to long, place closing bracket and opening brace ') {' in a separate line and place condition in separate line.
```php
if (
	... very long ...
	... multiline ...
) {
	...
}
else if (
	... very long ...
	... multiline ...
) {
	...
}
else {
	...
}
```

```php
for (
	... very long ...
	... multiline ...
) {
	...
}
```

```php
foreach (
	... very long ...
	... multiline ...
) {
	...
}
```

```php
while (
	... very long ...
	... multiline ...
) {
	...
}
```

```php
do {
	...
}
while (
	... very long ...
	... multiline ...
);
```

Don't forget the whitespace before opening bracket `(`.

Ternary operator
-----

When using ternary operator, put `then` and `else` part in separate lines and indent one time.
```php
// bad
$foo = $if ? $then : $else;

// good
$foo = $if
	? $then
	: $else;
```

Ternary operator is only allowed for primitive left/right decisions. Do not use in combination with function calls or other logic.
```php
// disallowed, use if-statement instead
$var = $if
	? $this->then()
	: $this->else();

// allowed
$var = $if
	? $then
	: $else;
```

Function/Method calls
=====

In function/method calls use space to split parameters, but do not add space before opening bracket `(`.
```php
// bad
func ('a','b','c');

// good
func('a', 'b', 'c');
```

When parameter list gets long, make it multiline and put **every** parameter in a single line.
Also put the closing bracket `)` in a single line.
```php
// bad
func ('short', 'short',
	'very long param');

// good
func(
	'short',
	'short',
	'very long param'
);
```

When chain method calls, put each method call in separate line if it make sense or the line gets to long.
```
// bad
$database->prepare('SELECT a,long,list,of,fields FROM some_table WHERE some_complex_and_long_where')->limit(100)->offset(50)->execute();

// a little bit better
$database->prepare('SELECT a,long,list,of,fields FROM some_table WHERE some_complex_and_long_where')
	->limit(100)->offset(50)->execute();

// good
$database
	->prepare('SELECT a,long,list,of,fields FROM some_table WHERE some_complex_and_long_where')
	->limit(100)
	->offset(50)
	->execute();
```

Variables
=====

Initialisation
-----

Make complex array initialisations multiline and put every element in single line.
```php
$array = array(
	'item1',
	'item2',
	'item3',
);
```

```php
$array = array(
	'index1' => 'item1',
	'index2' => 'item2',
	'index3' => 'item3',
);
```

Naming
-----

Use camel-case variable names.
Use significant variable names instead type prefix.
```php
// bad
$intUser = 1;

// good
$userId = 1;
```

```php
// bad
$objUser = ...some object...;

// good
$user = ...some object...;
```

```php
// bad
$arrUsers = array(1, 2, 3);

// good
$userIds = array(1, 2, 3);
```

```php
// bad
$arrUsers = array(...object..., ...object..., ...object...);

// good
$users = array(...object..., ...object..., ...object...);
```

Use camel-case method names.
```php
// bad
function foo_bar() {
}

// good
function fooBar() {
}
```

Use pascal-case class names.
```php
// bad
class foo_bar {
}

// good
class FooBar {
}
```

Logic
=====

Keep every line as simple as possible, do not combine to many operations in one line.
Use readable control structures if needed, even you write **a lot** more lines!
```php
// bad
$var = ($foo != $bar && count($zap) || $dig) ? $this->func($zap, $foo, $bar) : $this->other(count($zap))->chain($foo, $bar);

// good
$n = count($zap);
if (
	$foo != $bar && $n ||
	$dig
) {
	$var = $this->func($zap, $foo, $bar);
}
else {
	$var = $this->other($n)->chain($foo, $bar);
}
```

Don't use logic in function call parameters.
```php
// bad
func(
	$if
		? $then
		: $else,
	'foo',
	other('bar')
);

// good
$param = $if
	? $then
	: $else;
$other = other('bar');
func(
	$param,
	'foo',
	$other
);
```

Use function or method chaining with caution, but keep it as simple as possible.
```php
// avoid illogical chaining
// $this->func, strlen and file_get_contents does not logical belongs to each other
$this->func(strlen(file_get_contents('/some/file')));

// better
$content = file_get_contents('/some/file');
$this->func(strlen($content));

// best
$content = file_get_contents('/some/file');
$length  = strlen($content);
$this->func($length);
```

```php
// be gentle with logical chaining
// the array_* functions are logical belongs to each other,
// but its hard to understand this at a glance
$result = array_values(array_filter(array_map('trim', $array)));

// better
$result = array_values(
	array_filter(
		array_map(
			'trim',
			 $array
		 )
	)
);

// best, keep it simple, fast readable and more comprehensible - and it is short :-)
$result = array_map('trim', $array);
$result = array_filter($result);
$result = array_values($result);
```
