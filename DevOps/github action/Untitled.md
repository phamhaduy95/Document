git hub data can be obtained

```
github.event.issue.title
github.event.issue.body
github.event.pull_request.title
github.event.pull_request.body
github.event.comment.body
github.event.review.body
```


dangerous when they use input from the
aforementioned data points, controlled by users. The recommended approach for
mitigating code and command injection vulnerabilities in GitHub workflows involves
storing untrusted input as an intermediate environment variable. Here’s how you can
implement this best practice:

```
- name: Print Title
  env:
	TITLE: ${{ github.event.issue.title }}
	run: echo "$TITLE"

```

