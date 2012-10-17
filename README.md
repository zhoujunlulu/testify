Testify - Thou Shalt Write Tests
================================

Go code (golang) set of packages that provide many tools for testifying that your code will behave as you intend.

Features include:

  * [Easy assertions](#assert-package)
  * [Mocking](#mock-package)
  * [HTTP response trapping](#http-package)

Get started:

  * Install testify with [one line of code](#installation)
  * Read the API Documentation http://go.pkgdoc.org/github.com/stretchrcom/testify
  * To make your testing life easier, check out our other project, [gort](http://github.com/stretchrcom/gort)
  * For an introduction to writing test code in Go, see http://golang.org/doc/code.html#Testing
  * A little about [Test-Driven Development (TDD)](http://en.wikipedia.org/wiki/Test-driven_development)



`assert` package
----------------

The `assert` package provides some helpful methods that allow you to write better test code in Go.  Check out the [API documentation for the `assert` package](http://go.pkgdoc.org/github.com/stretchrcom/testify/assert).

  * Prints friendly, easy to read failure descriptions
  * Allows for very readable code
  * Optionally annotate each assertion with a message

See it in action:

    func TestSomething(t *testing.T) {
   
   	  // assert equality
      assert.Equal(t, 123, 123, "they should be equal")

      // assert inequality
      assert.NotEqual(t, 123, 456, "they should not be equal")

      // assert for nil (good for errors)
      assert.Nil(t, object)

      // assert for not nil (good when you expect something)
      if assert.NotNil(t, object) {

      	// now we know that object isn't nil, we are safe to make
      	// further assertions without causing any errors
        assert.Equal(t, "Something", object.Value)

      }

    }

  * Every assert func takes the `testing.T` object as the first argument.  This is how it writes the errors out through the normal `go test` capabilities.
  * Every assert func returns a bool indicating whether the assertion was successful or not, this is useful for if you want to go on making further assertions under certain conditions.

`http` package
--------------

The `http` package contains test objects useful for testing code that relies on the `net/http` package.  Check out the [API documentation for the `http` package](http://go.pkgdoc.org/github.com/stretchrcom/testify/http).

`mock` package
--------------

The `mock` package provides a mechanism for easily writing mock objects that can be used in place of real objects when writing test code.

An example test function that tests a piece of code that relies on an external object `testObj`, can setup expectations (testify) and assert that they indeed happened:

    package yours

    import (
      "testing"
      "github.com/stretchrcom/testify/mock"
    )

    /*
      Test objects
    */

    // MyMockedObject is a mocked object that implements an interface
    // that describes an object that the code I am testing relies on.
    type MyMockedObject struct{
      mock.Mock
    }

    // DoSomething is a method on MyMockedObject that implements some interface
    // and just records the activity, and returns what the Mock object tells it to.
    //
    // In the real object, this method would do something useful, but since this
    // is a mocked object - we're just going to stub it out.
    //
    // NOTE: This method is not being tested here, code that uses this object is.
    func (m *MyMockedObject) DoSomething(number int) (bool, error) {

      args := m.Mock.Called(number)
      return args.Bool(0), args.Error(1)

    }

    /*
      Actual test functions
    */

    // TestSomething is an example of how to use our test object to
    // make assertions about some target code we are testing.
    func TestSomething(t *testing.T) {

      // create an instance of our test object
      testObj := new(MyMockedObject)

      // setup expectations
      testObj.On("DoSomething", 123).Return(true, nil)

      // call the code we are testing
      targetFuncThatDoesSomethingWithObj(testObj)

      // assert that the expectations were met
      testObj.Mock.AssertExpectations(t)

    }

For more information on how to write mock code, check out the [API documentation for the `mock` package](http://go.pkgdoc.org/github.com/stretchrcom/testify/mock).

------

Installation
============

To install Testify, use `go get`:

    go get github.com/stretchrcom/testify

This will then make the following packages available to you:

    github.com/stretchrcom/testify/assert
    github.com/stretchrcom/testify/mock
    github.com/stretchrcom/testify/http

Import the `testify/assert` package into your code using this template:

    package yours

    import (
      "testing"
      "github.com/stretchrcom/testify/assert"
    )

    func TestSomething(t *testing.T) {

      assert.True(t, true, "True is true!")

    }

------

Contributing
============

Please feel free to submit issues, fork the repository and send pull requests!

When submitting an issue, we ask that you please include a complete test function that demonstrates the issue.  Extra credit for those using Testify to write the test code that demonstrates it.

------

Licence
=======
Copyright (c) 2012 Mat Ryer and Tyler Bunnell

Please consider promoting this project if you find it useful.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.