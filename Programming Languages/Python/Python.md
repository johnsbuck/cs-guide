---
tags:
  - python
  - overview
  - cheatsheet
---

# Python Cheatsheet
> [!INFO]- References
> [[References#^refbetaveroscheatsheet]]

## Script
### Creating a Python Script
When running a script, two things are useful to avoid issues:
1. Creating a `main()` function to hold any scripts/expressions you wish to run.
2. Ensuring you only run `main()` when running the Python script.
	- `if __name__ == "__main__": ...`

This ensures that you don't accidentally overwrite any variables, functions, or cause issues in other scripts.

### Example
```python
def main():
	print("Hello World")


if __name__ == "__main__":
	main()
```

**Output**
> Hello World!

## Unicode (-> [pydoc](https://docs.python.org/3/howto/unicode.html))
> [!INFO]- Tags
> #unicode #encoding

[PEP 0263](https://www.python.org/dev/peps/pep-0263/). First or second lines:

	`# coding=utf-8`

	`# -*- coding: utf-8 -*-`

	`# vim: set fileencoding=utf-8 :`

### [Codecs](https://docs.python.org/3/library/codecs.html)
```python
import codecs
codecs.encode(obj, encoding, errors)
codecs.decode(obj, encoding, errors)
```

can pass first 1 or first 2 arguments
encoding = "`ascii`" if omitted; alternatives: `utf_8`, `latin_1`, `big5` etc.
errors = "`strict`" if omitted; alternatives: `ignore`, `replace` etc.

## String (→ [std. methods](https://docs.python.org/3/library/stdtypes.html#string-methods), [module](https://docs.python.org/3/library/string.html))
> [!INFO]- Tags
> #string #format

-   `"formatstring".format(arg0, arg1, name=value)`
    -   Access things: `{}{0}{1[0]}{2.name}{name}`
    -   Generic: `{access:*>+#255,.3d}`
    -   shortcut: `{:0255}`
    -   space-pad: `{>255}`
    -   `foo = 0; bar = "baz"; "{foo} {bar}".format(**locals())` is fairly useful trick to quickly interpolate lots of local variables.
-   `str.translate(None, "setofchars")` deletes characters from set
-   `str.[lr]strip("setofchars")` from left/right/both sides
-   `str.replace(old, new)` exists (not regex)

## Regex (-> [pydoc](https://docs.python.org/3/library/re.html))
> [!INFO]- Tags
> #regex

`import re`

Special chars: `.^$*+?{}\[]|()`

-   `re.[search](https://docs.python.org/3/library/re.html#re.search)(pattern, string[, flags])` yields a `MatchObject` anywhere in string
-   `re.[match](https://docs.python.org/3/library/re.html#re.match)(pattern, string[, flags])` yields a `MatchObject` from start or None
-   `re.[fullmatch](https://docs.python.org/3/library/re.html#re.fullmatch)(pattern, string[, flags])` yields a `MatchObject` of the whole string or None (3.4+)
-   `re.[split](https://docs.python.org/3/library/re.html#re.split)(pattern, string[, maxsplit=0, flags=0])` yields a list of strings
-   `re.[findall](https://docs.python.org/3/library/re.html#re.findall)(pattern, string[, flags])` → a list of strings
-   `re.[finditer](https://docs.python.org/3/library/re.html#re.finditer)(pattern, string[, flags])` → iterator of `MatchObject`s
-   `re.[sub](https://docs.python.org/3/library/re.html#re.sub)(pattern, replacer, string[, count, flags])` returns a string. `replacer` is string (double-escape chars like `'\n'`, use `r'\1'` or `r'\g<123>'` or `r'\g<0>'` for a backref) or function mapping MatchObject to string

`[MatchObject](https://docs.python.org/3/library/re.html#match-objects).group(n)` gets the nth group. n = 0 or omitted means the whole match. Groups:

-   `(?: )` non-capturing
-   `(?# )` comment
-   `(?= )` lookahead / `(?! )` negative lookahead
-   `(?<= )` lookbehind / `(?<! )` negative lookbehind

## Sequences (-> [std.methods](https://docs.python.org/3/library/stdtypes.html)) & Dictionaries (-> [pydoc](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict))
> [!INFO]- Tags
> #dictionary #sequence #enumerate #list #iterate

<table>
	<tbody><tr><th>(python) <a href="https://docs.python.org/3/library/stdtypes.html">sequences</a></th><th>↓</th></tr>
	<tr><td>sorted(iterable, key = lambda x: x[1], reverse=True)</td><td>...</td></tr>
	<tr><td>max(iterable, key = lambda x: x[1])<br>min(iterable, key = lambda x: x[1])</td><td>...</td></tr>
	<tr><td>enumerate('ABCD', start=1)</td><td>(1, 'A'), (2, 'B')...</td></tr>
	<tr><th>(python) <a href="https://docs.python.org/3/library/stdtypes.html#mutable-sequence-types">mutable lists</a></th><th>↓</th></tr>
	<tr><td colspan="2">s.append(x)</td></tr>
	<tr><td colspan="2">s.extend(xs) <em>or</em> s += xs</td></tr>
	<tr><td colspan="2">s.insert(i, x)</td></tr>
	<tr><td>s.pop()<br>s.pop(0) <em>(from left)</em></td><td>popped element<br><code>pop</code> returns it in Python in general <small>(unlike some other languages)</small></td></tr>
	<tr><th>(python) <a href="https://docs.python.org/3/library/stdtypes.html#mapping-types-dict">dictionaries</a></th><th>↓</th></tr>
	<tr><td>d.keys()</td><td>dict_keys([k1, k2...])</td></tr>
	<tr><td>d.values()</td><td>dict_values([v1, v2...])</td></tr>
	<tr><td>d.items()</td><td>dict_items([(k1, v1)...])</td></tr>
	<tr><td>iter(d)</td><td>dict_keyiterator</td></tr>
	<tr><td>iter(d.values())</td><td>dict_valueiterator</td></tr>
	<tr><td>iter(d.items())</td><td>dict_itemiterator</td></tr>
	<tr><td colspan="2">d.clear()</td></tr>
</tbody></table>
-   Destructively updating a dictionary with another, or with a sequence of key-value pairs: `d1.update(d2)`.
-   Nondestructive idiom(?) for union-ing two dictionaries with string keys: `dict(d1, **d2)`. _3.9 ([PEP 0584](https://www.python.org/dev/peps/pep-0584/)):_ `d1 | d2`
- In both points above, when there are conflicting mappings for the same key, `d2` takes precedence.

## Sets ([std.doc](https://docs.python.org/3/library/stdtypes.html#set))
> [!INFO]- Tag
> #set

-   Element in set
	- `x in set_a`
	- `x not in set_a`
* Union of sets
	-  `set_a | set_b`
- Intersection of sets
	- `set_a & set_b`
- Difference of sets (in both)
	- `set_a - set_b`
- Symmetric difference of sets (not in both)
	- `set_a ^ set_b`
- Compound Assignment
	-  `set_a |= set_b`
	- etc.
- Add to set
	-  Add element: `set_a.add(x)`
	- Add set: `set_a.update(xs)`
- Remove from set
	- Remove element:  
		- `set_a.remove(x)` (KeyError if `x not in set_a`)
		- `set_a.discard(x)` (no error)
		-  `set_a.pop()` (arbitrary element)
	- Remove subset: `set_a -= xs`
- Clear set 
	- `set_a.clear()`

## Collections (-> [pydoc](https://docs.python.org/3/library/collections.html#collections.deque))
> [!INFO]- Tags
> #deque #Counter #collections #queue #defaultdict

### [deque](https://docs.python.org/3/library/collections.html#deque-objects)
Useful for making queues, especially for bread-first search.
* Append
	* Right-side: `.append(x)`
	* Left-side: `.appendleft(x)`
* Remove:
	* Right-side: `.pop(x)`
	* Left-side: `.popleft(x)`

> [!WARNING]+
> For concurrency/threaded programming, use the queue module.

### [Counter](https://docs.python.org/3/library/collections.html#collections.Counter)
* Counter(_[iterable-or-mapping-or-kwargs]_) 
	* e.g. Counter("aaaabb"), Counter(a=4, b=2)
* `.elements()`
	* Iterates over elements as many times as counted
		* ['a', 'a', 'a', 'a', 'b', 'b']
* `.update(_)`, `+`, `-` 
	* Treat like multisets
* `.most_common([n])`
	* Returns a list of the *n* most common elelments and their count from most common to least.
		* [('a', 4), ('b', 2)]
	* If _n_ is omitted or `None`, [`most_common()`](https://docs.python.org/3/library/collections.html#collections.Counter.most_common "collections.Counter.most_common") returns _all_ elements in the counter.
	* Elements with equal counts are ordered in the order first encountered.

### [defaultdict](https://docs.python.org/3/library/collections.html#collections.defaultdict)
* defaultdict(_[default_factory[, ...]]_)
	* will call default_factory() for default values, so pass e.g. int (for 0), list (for [])...

## Queue (-> [pydoc](https://docs.python.org/3/library/queue.html))
> [!INFO]- Tags
> #queue #multithreading #priorityqueue #lifo #fifo

Useful in threaded programming when information must be exchanged safely between multiple threads.
### [Queue](https://docs.python.org/3/library/queue.html#queue.Queue)
`queue.Queue(maxsize=0)`

Creates a **First-In, First-Out (FIFO)** queue.
  * `maxsize` is the upperbound of items that can be placed
	  *  The default, 0, sets the queue size to infinite.

### [LifoQueue](https://docs.python.org/3/library/queue.html#queue.LifoQueue)
`queue.LifoQueue(maxsize=0)`

Creates a **Last-In, First-Out (LIFO)** queue.
  * `maxsize` is the upperbound of items that can be placed
	  *  The default, 0, sets the queue size to infinite.

### [PriorityQueue](https://docs.python.org/3/library/queue.html#queue.PriorityQueue)
`queue.PriorityQueue(maxsize=0)`

Creates a priority queue.
  * `maxsize` is the upperbound of items that can be placed
	  *  The default, 0, sets the queue size to infinite.
  * Lowest values are retrieved first.
	  * If tuple or list entry, will take value from 0-index
		  * `sorted(list(entries))[0])`
		  * Typical pattern: `(priority_number, data)`

## os (-> [pydoc](https://docs.python.org/3/library/os.html))
> [!INFO]- Tags
> #os #operating-system #navigate #filesystem #file #read #write
* `os.chdir(pathstring)`
* `os.listdir(pathstring`
	* Unordered list of entries, no `.` or `..`
* `os.getcwd()`
* `os.path.join(*paths)`
	* Intelligently joins
	* [pydoc](https://docs.python.org/3/library/os.path.html)
* `os.rename(src, dst)`
	* File or dir
* `os.renames(src, dst)`
	* File or dir
	* Path directories allowed
* `os.walk(path, topdown=True, ...)` is a generator that walks all subdirectories. It yields a 3-tuple `(dirpath, dirnames, filenames)` for each directory. To do stuff, use `os.path.join(root, x)` for `x` and use You can modify `dirnames` in-place to inform or prune the walk.
	* [pydoc](https://docs.python.org/3/library/os.html#os.walk)
* `os.path.exists("foo")`
* `os.path.isfile("foo")
	* Exists and is a file
* `os.path.isdir("foo")`
	* Exists and is a directory

## subprocess (-> [pydoc](https://docs.python.org/3/library/subprocess.html))
> [!INFO]- Tags
> #subprocess #shell

```python
proc = subprocess.Popen("cat", shell=True, stdin=subprocess.PIPE, stdout=subprocess.PIPE)
(stdout_data, stderr_data) = proc.communicate(_[input=some_bytes]_)
```

One-shot, newer API:
```python
result = subprocess.run("cat", _[input=some_bytes,]_ stdout=subprocess.PIPE, stderr=subprocess.PIPE)
result.stdout # bytes or None
result.stderr # bytes or None
```

The first argument in both is `args: str|List[str]`.

-   If str: either simply the program to be run (if `shell=False`) or the whole shell line to lex and expand.
-   If List[str]: a list with first element the program to be run and remaining the string arguments to pass to it.

Add the `encoding='utf-8'` argument to Popen or run, and use unicode instead of bytes for both input and output.

On `run()` only, `capture_output=True` is short for `stdout=subprocess.PIPE, stderr=subprocess.PIPE`

## Itertools (-> [pydoc](https://docs.python.org/3/library/itertools.html))
> [!INFO]- Tags
> #itertools #permutation #combination

| (python) itertools.                      | ↓                           |
| ---------------------------------------- | --------------------------- |
| islice(\_, 9)                            | [:9] (hs: take 9)           |
| islice(\_, 2, 9)                         | [2:9] (hs: take 7 . drop 2) |
| islice(\_, 2, None)                      | [2:] (hs: drop 2)           |
| islice(\_, 2, 9, 3)                       | [2:9:3]                     |
| count(10)                                | 10, 11, 12, 13...           |
| count(10, 2)                             | 10, 12, 14, 16...           |
| cycle([31, 41, 59])                      | 31, 41, 59, 31, 41, 59...   |
| repeat(42)                               | 42, 42, 42...               |
| repeat(42, 5)                            | 42, 42, 42, 42, 42          |
| product('ABCD', 'PQRS')                  | AP, AQ, AR, AS, BP, BQ[...] |
| product('ABCD', repeat=2)                | AA, AB, AC, AD, BA, BB[...] |
| permutations('ABCD')                     | ABCD, ABDC[...]             |
| permutations('ABCD', 2)                  | AB, AC, AD, BA, BC[...]     |
| combinations('ABCD', 2)                  | AB, AC, AD, BC, BD, CD      |
| combinations_with_replacement('ABCD', 2) | AA, AB, AC, AD, BB[...]     |

## Random (-> [pydoc](https://docs.python.org/3/library/random.html))
> [!INFO]- Tags
> #random #uniform #gauss #choice #shuffle

| (python) random                          | ↓                                                      |
| ---------------------------------------- | ------------------------------------------------------ |
| randrange(...)                           | [start,] stop[, step], just like range (half-open)     |
| randint(lo, hi)                          | integers; inclusive                                    |
| choice(seq)                              | 1 sample                                               |
| choices(seq, ...)                        | k samples (w/ replacement), can weight etc. (see docs) |
| sample(seq, k)                           | k samples w/o replacement                              |
| shuffle(seq)                             | modifies in-place                                      |
| random()                                 | \[0, 1) float                                          |
| uniform(lo, hi)                          | \[lo, hi) float                                        |
| gauss(mu, sigma)normalvariate(mu, sigma) | normal distribution (latter slower but thread-safe)    |

## Other Standard Libraries
> [!INFO]- Tags
> #getpass #time

```python
import getpass
password = getpass.getpass()
```

```python
import time
time.sleep(0.5) # 0.5 seconds
```
