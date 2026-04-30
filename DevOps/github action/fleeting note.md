ta dùng `YAML` để define workflow
workflow là gì
component trong workflow:

- workflow job
	- workflow step
		- github action


trigger workflow thông qua:
- web hook triggers
- manual trigger
- schedule trigger

GitHub action:
viết bằng image hoặc javascript
reference locally hoặc image


ta có thể thêm các logic expression và for loop statement trong github action definition file


writing debug message
``` yaml
::debug::{message}
::warning file={name},line={line},endLine={el},title={title}::{message}  
::error file={name},line={line},endLine={el},title={title}::{message}
```

ta có thể sử dụng các parameter sau đây:
- `title`—A custom title for the message  
- `file`—The filename that raised the error or warning  
- `col`—The column/character number, starting at 1  
- `endColumn`—The end column number  
- `line`—The line number in the file starting with 1  
 - `endLine`—The end line number

Secret and variables
Làm sao để lưu Secret và variable tron


GitHub rest API để integrate vơi 

GitHub Runners are  
standalone instances that continuously ask GitHub if there is work for them to execute

GitHub-hosted runners


Note that in the action workflow, we use the option if: always(), which ensures that the test results are always added to the step summary. If we were to leave this to the defaults, no report would be added the moment any of the tests failed, since the previous step would produce an error and the workflow would be aborted.

sparse checkout
cho phép ta chỉ check out 1 một số file và directory nhất định trong 1 project thay vì toàn bộ project. Phương pháp này phù hợp với các project `monorepo`
keep local cache of download dependency from package manager
`actions/setup-node`, `actions/setup-python`

```yaml
- uses: actions/setup-node@v3
  with:
	node-version: 16
	cache: 'npm'
```

you can create a cache action 
```yaml
- uses: actions/cache@v3
  with:
	path: ~/.nuget/packages
	key: ${{ runner.os }}-nuget-${{ hashFiles('**/packages.lock.json') }}
	restore-keys: |
	${{ runner.os }}-nuget-
```
`hasFiles` function generate Hash value from a file.

detect cache hit
```yaml
- name: Generate file
  id: cache-file
	uses: actions/cache@v3
	with:
		path: file-location
		key: ${{ runner.os }}-file
```


adding caching
You can use GitHub’s cache action to cache dependencies for a job.


nên tìm hiểu về các nguồn trigger


các action phổ biến

```
actions/github-script@v3
actions/download-artifact@v3
```


grant permission to pull_request:
write, so we can set the comments on the PR using the script.

you can explore more about what data is included in GitHub event payload in this [doc](https://docs.github.com/en/webhooks/webhook-events-and-payloads#create)

l

standard directory structure for github workflow
where should I keep configuration files for workflow
