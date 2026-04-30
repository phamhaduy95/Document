
What does the react-testing-library offers?

A function that can render React component

A query function that

get

query element

A assert utility that works for

philosophy of testing: test with end-user preprestive, we should not care about inner execution. Behavior is consistent while implementation may be changed in future.

Including user with disabilities and handicaps:

About using data-testId. data-testId is hidden implementation

TODO list: things should be provided more details.

1. You may want to avoid testing the following implementation details:
    - Internal state of a component
    - Internal methods of a component
    - Lifecycle methods of a component
    - Child components
2. avoid using className as query for element. Prefer queryByRole, getAllByRole, need to mention about performance drop with getRole approach. Query provide more option to narrow down the element.
3. using within to make component as container so that you can query further its children.
4. use userEvent to simulate end-user action such as click, text input, hover and so on.
5. jsdom doesn't support layout. This means measurements like this will always return `0` as it does here.
6. mocking API call with mocked web service (new pages for it).
7. you can mock the implementation of one single function or entire module that SUT depends on. The mocking can be only applied to exported artifact. For internal use within module, mocking won’t work.
8. We can not test things that does not appear on html semetic such as animation, css since jsdom does not cover those.
9. should have only one assert for each test case. Style
10. 