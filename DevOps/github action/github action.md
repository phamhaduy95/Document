
javascript action trên trực tiếp trên runner và executed trên `nodejs`
javascript action khởi động nhanh hơn so với container

Docker container actions contain all their dependencies and are, therefore, very consistent. They allow you to develop your actions in any language

Docker container actions can reference an image in a container registry, like Docker
Hub or GitHub Packages.

ta có thể provide `url` của image trên docker hoặc cung cấp `DockerFile` trong field image của 1 step.

refer GitHub action bằng `use` statement

GitHub runner chỉ cho phép chạy 1 job per

https://github.com/orgs/community/discussions/26769

self-hosted runners

user có thể khai báo action theo 2 cách
docker container 
javascript

ta nên gom các actions và store trong 1 thư mục thuộc repo

các common action trong 1 workflow
tạo tự động 1 semantic version 

`CodeBuild` không hổ trợ run parallel mà ta cần define parallel action trên `CodePipeline`. mỗi action trigger 1 `CodeBuild` riêng

