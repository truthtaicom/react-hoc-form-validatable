`HOCInput` is the optional higher order component. 
If you know to work with React context you can rewrite it to work best with your app. 
Even though the component is good enough and you shouldn't rewrite it.

## Props in HOCInput

`HOCInput` has some props that you can send to start

* `name` {String} (Required) - Name of the input
* `rule` {String} Normal validation rules 
* `asyncRule` {String} Promise validation rule
* `defaultValue` {Any} Default value of the input
* `customErrorMessages` {String|Object} Custom error messages

Example 

```javascript


<Input      
  name="userName"
  defaultValue="abc"
  rule="notEmpty|minLength,4"
  asyncRule="asyncTestTrue,2000"
  customErrorMessages={{
      notEmpty: 'custom error for notEmpty',
      minLength: {
        en: 'custom error minLength {0} en',
        vi: 'custom error minLength {0} vi',
      },
  }}
/>

```

## Props send to wrapped component

`HOCInput` will wrap your component and send these props to your input component.


* `lang` {String} Language receive from the `HOCForm`
* `submitted` {Boolean} If the form submitting or not. We can use this to disable the form
* `validated` {Boolean} Check if the input is validated or not
* `value` {Any} The current value of the input
* `dirty` {Boolean} Check if the input is modified value
* `error` {Boolean} Check if the input fail the validation
* `pending` {Boolean} Check if the input is validating.
* `errorMessage` {String} Current error message of the input
* `defaultValue` {Any} Default value from props
* `name` {String} Name of the input
* `onBlur` {Function} Handler when user focus out the input
* `onChange` {Function} Handler when user change the value of the input

Props send to `HOCInput` also sent to your wrapped component.

```javascript
<Input      
  // Custom props will send to your wrapped component
  customProps="custom"
  //...
/>
```

Please see the example source to understand how to integrate with `HOCInput` 

