XSS cro
sanitize user input to filter out any malicious code
enable content security policy or CSP
with CSP we allow to predefine  the origin of our content
CSP dam bao bao brower

``` html
<head>
  <meta
    http-equiv="content-security-policy"
    content="
    script-src 'self' 'sha256-somehash';
    style-src 'self' 'sha256-somehash';
    "
  >
</head>
```

make a comparison

