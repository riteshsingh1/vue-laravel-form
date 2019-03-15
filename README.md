# vue-laravel-form
Vue Js Forms with laravel validation

![npm](https://img.shields.io/npm/v/@imritesh/form.svg)


This package provides a Form class that can validation our forms with laravel backend validation logic. The class is meant to be used with a Laravel back end.


## Install

You can install the package via yarn (or npm):
```bash
$ npm install @imritesh/form
```
```bash
$ yarn add @imritesh/form
```
By default, this package expects `axios` to be installed

```bash
$ yarn add axios
```

## Usage
```html
 <div class="form-group form-row is-invalid">
        <label for="form" class="col-md-3">Name</label>
        <input
          type="text"
          id="form"
          class="form-control col-md-9"
          v-model="form.code"
          :class="{ 'is-invalid': form.errors.has('code') }"
        >
        <span
          class="invalid-feedback col-md-9 offset-3"
          v-if="form.errors.has('code')"
          v-text="form.errors.first('code')"
        ></span>
</div>
```
```js

import Form from 'form-backend-validation';

// Instantiate a form class with some values
const form = new Form({
    field1: 'value 1',
    field2: 'value 2',
    person: {
        first_name: 'John',
        last_name: 'Doe',
    },
});

// A form can also be initiated with an array
const form = new Form(['field1', 'field2']);

// Submit the form, you can also use `.put`, `.patch` and `.delete`
form.post(anUrl)
   .then(response => ...)
   .catch(response => ...);

// Returns true if request is being executed
form.processing;

// If there were any validation errors, you easily access them

// Example error response (json)
{
    "errors": {
        "field1": ['Value is required'],
        "field2": ['Value is required']
    }
}

// Returns an object in which the keys are the field names
// and the values array with error message sent by the server
form.errors.all();

// Returns true if there were any error
form.errors.any();

// Returns true if there is an error for the given field name or object
form.errors.has(key);

// Returns the first error for the given field name
form.errors.first(key);

// Returns an array with errors for the given field name
form.errors.get(key);

// Shortcut for getting the errors for the given field name
form.getError(key);

// Clear all errors
form.errors.clear();

// Clear the error of the given field name or all errors on the given object
form.errors.clear(key);

// Returns an object containing fields based on the given array of field names
form.only(keys);

// Reset the values of the form to those passed to the constructor
form.reset();

// Set the values which should be used when calling reset()
form.setInitialValues();

// Populate a form after its instantiation, the populated fields won't override the initial fields
// Fields not present at instantiation will not be populated
const form = new Form({
    field1: '',
    field2: '',
});

form.populate({
    field1: 'foo',
    field2: 'bar',
});

```

### Options

The `Form` class accepts a second `options` parameter.

```js
const form = new Form({
    field1: 'value 1',
    field2: 'value 2',
}, {
    resetOnSuccess: false,
});
```

You can also pass options via a `withOptions` method (this example uses the `create` factory method.

```
const form = Form.create()
    .withOptions({ resetOnSuccess: false })
    .withData({
        field1: 'value 1',
        field2: 'value 2',
    });
```

#### `resetOnSuccess: bool`

Default: `true`. Set to `false` if you don't want the form to reset to its original values after a succesful submit.