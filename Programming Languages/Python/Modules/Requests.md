---
tags:
  - requests
  - rest
  - api
  - python
  - web
---

# Requests (-> [doc](https://docs.python-requests.org/en/latest/))
> [!INFO]- References
> [[References#^refbetaveroscheatsheet]]

```python
import requests
r = requests.get('http://www.example.com/', params={'foo': 'bar'}) # values can also be lists
r = requests.post('http://www.example.com/', data={'foo': 'bar'}) # auto form-encodes; str/bytes is OK too
```

r = requests.request('VERB', url, ...) # 'GET' or 'POST' or others, even custom

Other parameters include:

-   `cookies={'foo': 'bar'}`
-   `headers={'user-agent': 'my-app/0.0.1'}`
-   `json={'foo': 'bar'}` (`data=` its JSON representation; some APIs take it)

```python
r.url # see which URL was requested, after redirects; includes the encoded params you passed
  r.history # Response objects for any redirects that occurred
r.status_code
  r.raise_for_status() # raise exception if 4xx or 5xx
r.text # automagical unicode
  r.content # bytes
  r.json() # parse as JSON
r.headers # of response; magical dict with case-insensitive keys
r.cookies # of response
```
Make a session to store cookies:

```python
sess = request.Session()
sess.get('http://www.example.com/') # as before
```
