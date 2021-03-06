The first thing before we started we have to define validation rules. In this library, rules just store in a simple object.
 The library provided some simple default rules (I haven't had much time to write more, so you could extend the default 
 rules and write your own).

## Overview

Below is an example of validation rules.
  
```javascript
const ruleObDefault = {
  isEmail: {
    rule: /^([a-zA-Z0-9_\.\-])+\@(([a-zA-Z0-9\-])+\.)+([a-zA-Z0-9]{2,4})+$/,
    message: {
      // The library support object key value message
      error: {
        en: 'This field must be a valid email address.',
        vi: 'Hãy nhập một địa chỉ email hợp lệ vào ô này',
      },
    },
  },

  notEmpty: {

    rule(value) {
      return (!(value === null || value === ''));
    },
    
    // If you just want send a simple string not a object, do it! 
    message: {
      error: 'This field must be not empty',
    },
  },

  minLength: {

    rule(value, param) {
      return value.length >= param[0];
    },

    message: {
      error: {
        en: 'This field length must be at least {0} character',
        vi: 'Ô này phải chứa ít nhất {0} ký tự',
      },
    },

  },
  
  //... more rules
};

```
Each rule in rules object is also a object with these keys `rule` and `message`.

We pass the rule object to the form via `rules` prop so the library can lookup the rules

The library has the built in validation rules. You can extend it or write your own validation rules

```javascript
import {defaultRules} from 'react-hoc-form-validatable';

<Form rules={defaultRules}>
  //...
</Form>

```

## Types

Currently the validation rule support 3 types of validation

* Regex

If the input value passes the regex test so the input will pass the validation rule.

```javascript

// Example with regex rule.
isEmail: {
    rule: /^([a-zA-Z0-9_\.\-])+\@(([a-zA-Z0-9\-])+\.)+([a-zA-Z0-9]{2,4})+$/,
},


```

* Function

The library when validating a function rule will call the function in `rule` property. 

The function rule will be applied with two parameters.

The first is the input value, the second is the rule params.
If the rule is pass to the input like this `minLength,3,4` the second param will be an array with the value is ['3','4'].
With the parameters, you can calculate if the value can pass the rule or not. The function returns true, it means the 
input value is passed the rule and otherwise if return false.


```javascript

// Example with function rule.
minLength: {

    rule(value, param) {
      return value.length >= param[0];
    },
}
```

* Promise

Similar to function rule, but the different thing is you have to return a Promise that resolves a Boolean value.

This is a place you will perform an asynchronous validation like calling to the API...

The Promise polyfill in your app has to support Promise.cancel(). You can use BlueBird for that. 
But if you don't use BlueBird you can use the wrapper come with the library.

```javascript
import {cancelAblePromise} from 'react-hoc-form-validatable';

// Example with function rule.
asyncTestTrue: {
    rule: (value, params) => cancelAblePromise(new Promise((resolve) => {
      setTimeout(() => { resolve(true); }, 2000);
    }))
 }

```

## Messages

When the input value is not passed the validation rule. The input will receive formatted message with the prop `errorMessage`.

We have to add error message for each rule via `message` property


```javascript

minLength: {

    rule(value, param) {
      return value.length >= param[0];
    },

    message: {
      error: {
        en: 'This field length must be at least {0} character',
        vi: 'Ô này phải chứa ít nhất {0} ký tự',
      },
    },

  },
```


The error object will look language provided in the form

```javascript

<FormDemoBasic 
    validateLang="en"
    //...
/>

```

You can use `{#number}` to replace with param

Example 'minLength,6' => 'This field length must be at least 6 character'

## Usage in the input

Each key in the object rules is a rule name that you can pass to input by `rule` or `asyncRule` prop in the input


```javascript

// single rule
<Input
  //... other props
  rule="notEmpty"
/>

// multiple rules with param
<Input
  //... other props
  rule="notEmpty|minLength,4"
  asyncRule="asyncTestTrue,2000"
/>


```

Multiple rules separated by '|' character. So the input has 2 validation rules are notEmpty and minLength

In rule minLength,4, the first group minLength is a rule name, after that is a param.

So the rule will pass to minLength function validation rule(value, param) {}.

With value current input value. param is Array ['4']. If minLength,4,5 param will be ['4','5']

The input also has Promise async validation rule. Currently async validation only take 1 rule at the time.

It will be madness if support more than 2 rules in async validation 