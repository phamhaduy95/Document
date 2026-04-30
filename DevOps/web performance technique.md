optimize DNS

If the JS execution order is not critical and it must be run before the onload event
triggers, then set the “async”9 attribute, as in:
``` html
<script async src=”/js/myfile.js”>.
```

- `async`: Load in parallel, execute as soon as downloaded, no guaranteed order.
- `defer`: Load in parallel, execute after HTML parsing, guaranteed order.