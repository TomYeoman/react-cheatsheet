# react-cheatsheet
Contains everything react

- [Test structure](#test-structure)
- [Function testing](#function-testing)
  - [Spies VS Stubs](#spies-stubs)
- [Matchers](#matchers)
  - [Basic matchers](#basic-matchers)
  - [Truthiness](#truthiness)
  - [Numbers](#numbers)
  - [Strings](#strings)
  
  
  ##function-testing
  
  There are several ways we can test whether a function prop is called as expected
  
  ###spies-stubs
  
  *Spies*
  
  Creates a mock function but also tracks calls to object[methodName]  
  
  ```
  jest.spyOn(object, methodName) ( See https://jestjs.io/docs/en/jest-object#jestspyonobject-methodname )
  sinon.spyOn(MyClass.prototype, 'onClick');
  jasmine.createSpy()
  chai.spy()
  ```

  Spies act exactly like the original method, except with the ability to access all the data about calls
  
  ```
  sinon.spy(jQuery, "ajax");

  "test should inspect jQuery.getJSON's usage of jQuery.ajax": function () {
      jQuery.getJSON("/some/resource");

      assert(jQuery.ajax.calledOnce);
      assertEquals("/some/resource", jQuery.ajax.getCall(0).args[0].url);
      assertEquals("json", jQuery.ajax.getCall(0).args[0].dataType);
  }
    
  ```
  
  *Stubs / Mocks*
  
  These are functions which have a mock property with some data on it. Several libraries provide different API's for this functionality, E.g

  ```
  jest.fn()
  sinon.stub()
  ```
  
  We can interaction with this stubbed function like so
  
  ```
  const mockCallback = jest.fn();
  forEach([0, 1], mockCallback);

  // The mock function is called twice
  expect(mockCallback.mock.calls.length).toBe(2);

  // The first argument of the first call to the function was 0
  expect(mockCallback.mock.calls[0][0]).toBe(0);

  // The first argument of the second call to the function was 1
  expect(mockCallback.mock.calls[1][0]).toBe(1);

  // The return value of the first call to the function was 42
  expect(mockCallback.mock.results[0].value).toBe(42);

  ```
  They are also useful for injecting test values into a function
  
  
  ```
  const myMock = jest.fn();
  console.log(myMock());
  // > undefined

  myMock
    .mockReturnValueOnce(10)
    .mockReturnValueOnce('x')
    .mockReturnValue(true);

  console.log(myMock(), myMock(), myMock(), myMock());
  // > 10, 'x', true, true
  ```
  
  More info : https://jestjs.io/docs/en/mock-functions
