ngăn chặn các request từ author lạ

MR should be approved by me and merged in order for workflow to run

set permissions

store secret in secure location

detect vulnerabilities 

#### echo shell injection

**echo CLI injection** refers to a potential security vulnerability where an application uses the `echo` command to display user-supplied data without proper sanitization, allowing an attacker to inject and execute arbitrary operating system commands


#### branch protection policy
Để tạo mới 1 rule và apply cho repository ta làm theo hướng dẫn chính thức của github.

Các rule quan trọng nên áp dụng:
1. MR trước khi merge cần đảm bảo:
	- số lượng approval cần thiết
	- toàn bộ comment được resolved hết trước khi merge
	- up to date với current ref
2. 

create pull request template
