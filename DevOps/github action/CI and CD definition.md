Continuous integration (CI) is a DevOps practice, in which you regularly merge code changes into the central repository and run automated builds and tests to check the
correctness and quality of the code. CI aims to provide rapid feedback and identify and correct defects as soon as possible. CI relies on the source code version control
system to trigger builds and tests at every commit.

validate a piece of code can be committed into source code
validate the quality of source code (linting, type checking, unit testing)
security checking
packaging 

```yaml
- uses: actions/checkout@v3
  with:
	ref: 'main' #not naming the ref will fetch the default branch
	fetch-depth: '1' #1 is default and 0 fetches the full depth of the repo
```


Caching strategy

When you share the key between jobs, you can use the cached artifacts and nicely separate the concerns of CI and the steps to create the final output for delivery.

we should allow testin