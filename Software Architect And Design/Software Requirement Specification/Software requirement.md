apply Gherkin template for writing requirement specification
Gherkin common structure
use keyword to separate into sub-contents
- Feature
- Description (optional) give more detail about feature for rule. You can make it as free text under Feature and Rule part
- Rule: 
- Scenario or Example
	- Each line of scenario belong to those segments: 
	- As 
	- When
	- Then
	- And 
=> Show example:
A `Background` allows you to add some context to the scenarios that follow it. It can contain one or more `Given` steps, which are run before _each_ scenario.

Example

In addition to being a specification and documentation, an example is also a _test_. As a whole, your examples are an _executable specification_ of the system.

Examples follow this same pattern:

- Describe an initial context (`Given` steps)
- Describe an event (`When` steps)
- Describe an expected outcome (`Then` steps)
Keywords are not taken into account when looking for a step definition. This means you cannot have a `Given`, `When`, `Then`, `And` or `But` step with the same text as another step.

`Given`: to describe the context such the user, what tool is 


Given the following users exist: 

| name   | email              | twitter         |
| ------ | ------------------ | --------------- |
| Aslak  | aslak@cucumber.io  | @aslak_hellesoy |
| Julien | julien@cucumber.io | @jbpros         |
| Matt   | matt@cucumber.io   | @mattwynne      |

``` txt
Feature: Some important feature
  Scenario: Do not show balance if not logged in
    Given I am not logged on to the mobile banking app
    When I open the mobile banking app
    Then I can see a login page
    And I do not see account balance

  Scenario: Show balance on the accounts page after logging in
    Given I have just logged on to the mobile banking app
    When I load the accounts page
    Then I can see account balance for each of my accounts
```


Data selector where user can browse tags and drag/drop it into control to associate tag data with the control. 
user click on filter view

Project Management dùng excel
Excel là công cụ chính record các thông tin cũng như status của dự án.

Q/A sessions
Tạo và export 1 file excel lên SharePoint. Khi có câu hỏi nào về business và requirement, developer thêm câu hỏi vào file và chờ câu trả lời từ stakeholder. 
Cách làm này đem lại 2 lợi ích chính
- Tài liệu chung để các bên có thể tham khảo khi cần tìm hiểu về requirement.
- Dùng như một evidence khi có incident

Task management spreadsheet

Track tiến độ công việc bằng excel
task meta data (user stories) 
sub-tasks
