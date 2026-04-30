làm sao ta lưu được artifact sau mỗi step và nếu lưu được thì ta sẽ store ở đâu ?
khi workflow được hoàn thành, các artifact có tự động xóa hay không
Enable cache data như thế nào để đẩy nhanh quá trình build


tiến hành benchmark 
tính chi phí cho mỗi lần run
thử build sử dụng nhiều phương pháp khác nhau.


This example shows why it can be worthwhile to have sequential steps in a job, instead
of running everything in parallel jobs. Running everything in parallel can save you time,
to get feedback faster back to a developer, but it can also cost more action minutes.


```yaml
on:
  pull_request:
    types:
      - closed

jobs:
  if_merged:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
    steps:
    - run: |
        echo The PR was merged
```