---
tags:
  - struct
  - python
  - c
  - c++
---

# Struct (-> [pydoc](https://docs.python.org/3/library/struct.html))
> [!INFO]- References
> [[References#^refbetaveroscheatsheet]]

```python
struct.pack(fmt, v1, v2, ...)
struct.unpack(fmt, bytes) # always tuple, possibly length-1
struct.calcsize(fmt) # number of bytes
```

First character of fmt optionally indicates endianness/size/alignment. You probably just want one of:
* *< little-endian (x86)
* *\> big-endian ("natural writing")

Remaining characters format things one at a time. Uppercase variants indicate the things in brackets, usually `unsigned`. Most useful:
-   `'<I'` packs/unpacks 32-bit unsigned ints. `struct.pack('<I', num)`
-   `'<Q'` packs/unpacks 64-bit unsigned ints. `struct.pack('<Q', num)`

Precede with a count a la `vi` to repeat: `'4h'` means `'hhhh'`. Exception: for `'s'` the count means the length of the string. (Pascal-style strings are packed with the first byte indicating the length of the string.)

<table>
	<tbody><tr><th>format</th><th>C type</th><th>Python type</th><th># bytes</th></tr>
	<tr><td>x</td><td colspan="3" class="empty"></td></tr>
	<tr><td>c</td><td>char</td><td>bytes (len = 1)</td><td>1</td></tr>
	<tr><td>b/B</td><td>[unsigned] char</td><td>int</td><td>1</td></tr>
	<tr><td>?</td><td>_Bool</td><td>bool</td><td>1</td></tr>
	<tr><td>h/H</td><td>[unsigned] short</td><td>int</td><td>2</td></tr>
	<tr><td>i/I</td><td>[unsigned] int</td><td>int</td><td>4</td></tr>
	<tr><td>l/L</td><td>[unsigned] long</td><td>int</td><td>4</td></tr>
	<tr><td>q/Q</td><td>[unsigned] long long</td><td>int</td><td>8</td></tr>
	<tr><td>n/N</td><td>[s]size_t</td><td>int</td><td class="empty"></td></tr>
	<tr><td>e</td><td class="empty"></td><td>float</td><td>2</td></tr>
	<tr><td>f</td><td>float</td><td>float</td><td>4</td></tr>
	<tr><td>d</td><td>double</td><td>float</td><td>8</td></tr>
	<tr><td>s/p</td><td>char[] [Pascal style]</td><td>bytes</td><td class="empty"></td></tr>
	<tr><td>P</td><td>void *</td><td>int</td><td class="empty"></td></tr>
</tbody></table>
