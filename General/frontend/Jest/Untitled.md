Jest is unit testing framework for both back-end and front-end JavaScript application. Unit testing often targets functions or small modules.

Install jest using npm or yarn

To create test case use test(“testing description”, ()=>{

});

**Matcher**

Note: Jest use nodejs as an runtime environment, so any knowledge about node is valuable.

Exact matcher

toBe() use Object.is as comparison method.

for checking every property value of object. Use toEqual or use toStrictEqual

You can check if an array or iterable contains a particular item using toContain

to check whether one method throw correct Error object when it is called, use toThrow

for string type, use toMatch

**Mocking module**

ll mock functions have this special `.mock` property, which is where data about how the function has been called and what the function returned is kept.

Mock function is check whether a function works as expected. These includes number of time it is called, argument for each call and it return value.

_// The function was called exactly once_  
expect(someMockFunction.mock.calls.length).toBe(1);  
  
_// The first arg of the first call to the function was 'first arg'_  
expect(someMockFunction.mock.calls[0][0]).toBe('first arg');  
  
_// The second arg of the first call to the function was 'second arg'_  
expect(someMockFunction.mock.calls[0][1]).toBe('second arg');  
  
_// The return value of the first call to the function was 'return value'_  
expect(someMockFunction.mock.results[0].value).toBe('return value');  
  
_// The function was called with a certain `this` context: the `element` object._  
expect(someMockFunction.mock.contexts[0]).toBe(element);  
  
_// This function was instantiated exactly twice_  
expect(someMockFunction.mock.instances.length).toBe(2);  
  
_// The object returned by the first instantiation of this function_  
_// had a `name` property whose value was set to 'test'_  
expect(someMockFunction.mock.instances[0].name).toBe('test');  
  
_// The first argument of the last call to the function was 'test'_  
expect(someMockFunction.mock.lastCall[0]).toBe('test');

**Mock Return Values**[**​**](https://jestjs.io/docs/mock-functions#mock-return-values "Direct link to heading")

You can provide a false return value for mocked function

const myMock = jest.fn();  
console.log(myMock());  
_// > undefined_  
  
myMock.mockReturnValueOnce(10).mockReturnValueOnce('x').mockReturnValue(true);  
  
console.log(myMock(), myMock(), myMock(), myMock());  
_// > 10, 'x', true, true_

Mocking is to provide fake data so that test don’t need to depend on

Now, in order to test this method without actually hitting the API (and thus creating slow and fragile tests), we can use the `jest.mock(...)` function to automatically mock the axios module.

Some third-party library has mocking with jest.

- How to create mocking for function

You can mock separated subset instead of entire module.

  // Mission

Design and develop unit testing for any created data structures.

- Which will be tested?

- How to organize test?Jest is unit testing framework for both back-end and front-end JavaScript application. Unit testing often targets functions or small modules.

Install jest using npm or yarn

To create test case use test(“testing description”, ()=>{

});

**Matcher**

Note: Jest use nodejs as an runtime environment, so any knowledge about node is valuable.

Exact matcher

toBe() use Object.is as comparison method.

for checking every property value of object. Use toEqual or use toStrictEqual

You can check if an array or iterable contains a particular item using toContain

to check whether one method throw correct Error object when it is called, use toThrow

for string type, use toMatch

**Mocking module**

ll mock functions have this special `.mock` property, which is where data about how the function has been called and what the function returned is kept.

Mock function is check whether a function works as expected. These includes number of time it is called, argument for each call and it return value.

_// The function was called exactly once_  
expect(someMockFunction.mock.calls.length).toBe(1);  
  
_// The first arg of the first call to the function was 'first arg'_  
expect(someMockFunction.mock.calls[0][0]).toBe('first arg');  
  
_// The second arg of the first call to the function was 'second arg'_  
expect(someMockFunction.mock.calls[0][1]).toBe('second arg');  
  
_// The return value of the first call to the function was 'return value'_  
expect(someMockFunction.mock.results[0].value).toBe('return value');  
  
_// The function was called with a certain `this` context: the `element` object._  
expect(someMockFunction.mock.contexts[0]).toBe(element);  
  
_// This function was instantiated exactly twice_  
expect(someMockFunction.mock.instances.length).toBe(2);  
  
_// The object returned by the first instantiation of this function_  
_// had a `name` property whose value was set to 'test'_  
expect(someMockFunction.mock.instances[0].name).toBe('test');  
  
_// The first argument of the last call to the function was 'test'_  
expect(someMockFunction.mock.lastCall[0]).toBe('test');

**Mock Return Values**[**​**](https://jestjs.io/docs/mock-functions#mock-return-values "Direct link to heading")

You can provide a false return value for mocked function

const myMock = jest.fn();  
console.log(myMock());  
_// > undefined_  
  
myMock.mockReturnValueOnce(10).mockReturnValueOnce('x').mockReturnValue(true);  
  
console.log(myMock(), myMock(), myMock(), myMock());  
_// > 10, 'x', true, true_

Mocking is to provide fake data so that test don’t need to depend on

Now, in order to test this method without actually hitting the API (and thus creating slow and fragile tests), we can use the `jest.mock(...)` function to automatically mock the axios module.

Some third-party library has mocking with jest.

- How to create mocking for function

You can mock separated subset instead of entire module.

  // Mission

Design and develop unit testing for any created data structures.

- Which will be tested?

- How to organize test?