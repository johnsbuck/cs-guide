# Encoding  Bytes & Ints
> [!INFO]- References
> [[References#^refbetaveroscheatsheet]]

```python
b'foo'.[hex](https://docs.python.org/3/library/stdtypes.html#bytes.hex)([sep[, bytes_per_sep]]) # 3.5+
bytes.[fromhex](https://docs.python.org/3/library/stdtypes.html#bytes.fromhex)('666f6f') # 3.0+

# 3.2+
(some_int).[to_bytes](https://docs.python.org/3/library/stdtypes.html#int.to_bytes)(n, 'big'|'little'[, signed=False])
	n = (some_int.bit_length() + 7) // 8
# static method:
int.[from_bytes](https://docs.python.org/3/library/stdtypes.html#int.from_bytes)(some_bytes, 'big'|'little'[, signed=False])

# [base64](https://docs.python.org/3/library/base64.html) module: bytes to bytes (for greppability: b64encode, b64decode)
base64.b64{encode,decode}(s, altchars='+/') # url '-_'
base64.{standard_b64,urlsafe_b64,b32,b16,a85,b85}{encode,decode}
```

As before: little-endian is how you'll see it in e.g. x86, big-endian is "natural writing".