---
layout: single
title:  "Unit Testing for Beginners with JavaScript and Jest"
date:   2018-07-09 18:33:52 +0900
categories: jekyll update
---

This article is for those who barely knows about Unit Testing or those who just started working on Unit Testing. I will discuss what Unit Testing is, why it is needed and how to do some initial testing. You will get familiar with Unit Testing after reading this article and will be able to start testing your code.

[What is Unit Testing?]

Unit Testing is a way of testing our code to see if it works properly (or if it returns intended output or updates some states or UI) as intended. You probably have been doing it everyday, but manually by printing out console messages or clicking and navigating around. If you have done it in this way, that means you have done manual testing. There is opposite way to test your code and it is automated testing. Unit Testing is one type of automated testing. Automated testing can save us a lot of time and and increase code reliability if you design and implement the test well.    


[Are there any other types of automated testing?]
Integrated Testing is another type of automated testing. The difference between Unit Testing and Integrated Testing is Unit Testing normally doesn't involves with  external dependency, modules or functions, but Integrated Testing involves with external dependencies like database and http request (external modules). With Unit Testing, you can find a bug or error earlier in the development process than Integrated Testing, but you will not catch integration errors or broader system-level errors. Either type of testing has its own pros and cons and it is our job, as a developer, to decide how to design and implement test cases for less error and more code reliability. And here we will discuss the basics of Unit Testing.

[Which Frameworks?]
There are great frameworks for Unit Testing. Jasmine[https://github.com/jasmine/jasmine], Mocha[https://mochajs.org/], Jest [https://jestjs.io/] are the most popular frameworks. Mocha is the most popular one but I will use Jest because I am using React and Jest is used by Facebook developers when working with React projects.


[Tech Stack]
Node.js
NPM
Jest

To see if Node is installed, open the Windows Command Prompt or Mac Terminal (or a similar command line tool), and type node -v in Terminal. This should print a version number, so you’ll see something like this v0.10.35.

To see if NPM is installed, type npm -v in Terminal. This should print NPM’s version number so you’ll see something like this 5.6.0.

(or else, install Node and NPM.)

Now you create a new directory called 'unit-testing' and create a new package.json file by doing npm init in the directory. Then you need to install Jest using yarn or npm

```
yarn add --dev jest
npm install --save-dev jest
```

If you add '-dev' in the command, It will be installed under 'dev-dependencies' in the package.json file and will not be included production version of your app later.


[First Test]

Open your IDE and you will see a folder structure like this. Then, create an index.js file and a test.js file. In the index.js, we will add a function (unit) and we will write some code in the test.js file to test the function.     


Let's add a simple math function to the index.js and add a test to the test.js, just like official Jest introduction https://jestjs.io/docs/en/getting-started

```
function sum(a, b) {
  return a + b;
}
module.exports = sum;
```

```
const sum = require('./index');

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

And then, type jest in the console, you will see

image  

As you can see #1 describes what the test is about, #2 describes the function being tested with arguments, and #3 is a matcher to see if #2 returns intended output. If there was something wrong in your code, it will log the error and you can debug it.

When designing and implementing a test, it's important to note that the test should be reasonable, not too specific or too general to ensure our code works as expected and reliable. Below example has an empty space after 'Welcome', and if you forget about the space in the test code, it will produce an error log.  

```
module.exports.greet = function(name) {
  return 'Welcome ' + name;
}


describe('greet', () => {
  it('should return the greeting message', () => {
    const result = lib.greet('Moogun')
    expect(result).toBe('Welcome Moogun')
  })
})
```

So there are various types of matchers other than toBe() and let's see how different they are for strings, objects, arrays, Truthiness, Exceptions and more by testing a few different matchers  


[Matchers]

Matchers let you test values in different ways and some commonly used matchers are toBe() to test exact equality, toMatch() to test against regular expressions with, toContain() to check if an array contains a particular item, toEqual() to check every field of an object or array, toBeDefined() to check if a something is defined (i.e function).


For numbers, you can use  
```
test('two plus two', () => {
  const value = 2 + 2;
  expect(value).toBeGreaterThan(3);
  expect(value).toBeGreaterThanOrEqual(3.5);
  expect(value).toBeLessThan(5);
  expect(value).toBeLessThanOrEqual(4.5);

  // toBe and toEqual are equivalent for numbers
  expect(value).toBe(4);
  expect(value).toEqual(4);
});
```

```
describe('currency', () => {
  it('supported currency', () => {
    const result = lib.getCurrencies()
    expect(result).toBeDefined()
    expect(result).not.toBeNull()


    expect(result[0]).toBe('USD')
    expect(result[1]).toBe('AUD')
    expect(result[2]).toBe('EUR')
    expect(result.length).toBe(3)

    //proper way
    expect(result).toContain('USD')
    expect(result).toContain('AUD')

    //Ideal way
    expect(result).toEqual(expect.arrayContaining(['USD', 'AUD', 'EUR']))
  })
})
```

```
describe('get product', () => {
  it('should return product', () => {
    const result = lib.getProduct(1)
    expect(result).toBe({ id: 1, price: 10 })
  })
})
```

```
module.exports.getProduct = function(productId) {
  return { id: productId, price: 10 };
}


describe('get product', () => {
  it('should return product', () => {
    const result = lib.getProduct(1)
    expect(result).toEqual({ id: 1, price: 10 })
    expect(result).toMatchObject({ id: 1, price: 10 })
  })
})
```

```
describe('get product', () => {
  it('should return product', () => {
    const result = lib.getProduct(1)
    // expect(result).toEqual({ id: 1, price: 10 })
    // expect(result).toMatchObject({ id: 1, price: 10 })
    expect(result).toHaveProperty('id', 1)
  })
})
```


```
describe('register user', () => {
  it('should through if username is falsy ', () => {
    //falsy // Null, undefined, Nan, 0, false
    expect(() => { lib.registerUser(null) }))
  })
})
```

```
describe('register user', () => {
  it('should through if username is falsy ', () => {
    //falsy // Null, undefined, Nan, 0, false
    const args = [null, undefined, NaN, '', 0, false]
    args.forEach(a => {
      expect(() => { lib.registerUser(a) }).toThrow()
    })
  })
  it('should return a user object if valid username is passed', ()=> {
    const result = lib.registerUser('Moogun')
    expect(result).toMatchObject({username: 'Moogun'})
    expect(result.id).toBeGreaterThan(0)
  })
})

```

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.

function hello(name) {
  console.log(name)
}
{% endhighlight %}


Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
