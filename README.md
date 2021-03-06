# vue-form-validator

Deadly simple form validation for Vue.js. Features:

- Built in data types, including email, number, domain, url, etc.
- Customizable validation rules.
- Native support for async validation.

## Usage

To use `vue-form-validator`, you need to `install` it to `Vue`.

```javascript
import Vue from 'vue';
import VueValidator from 'vue-form-validator';
Vue.use(VueValidator);
```

Then you can use `v-validator` directive on `form` element:

```html
<div id="app">
    <form class="form" v-validator="loginForm">
        <div class="field">
            <label>Username:</label>
            <input type="text" v-model="userName" required minlength="3">
        </div>
        <div class="field">
            <label>Password:</label>
            <input type="password" v-model="password" minlength="6">
        </div>
    </form>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        userName: '',
        password: ''
    }
});
```

The value for `v-validator` attribute is required. You can use it to check the validability of the form. For the code above, `vm.loginForm.$valid` means whether the form is valid.

### Validation rules

Validation rules are attributes added to `input`/`select`/`textarea` elements. For example, `<input type="text" v-model="userName" required>` means the `input` is required to fill.

#### required

The `required` rule indicates the form control must be filled or checked. Add `required` attribute to the form control without attribute value.

```html
<input type="text" v-model="title" required>
```

#### minlength

The `minlength="x"` rule means the form control must be filled with at least `x` characters.

```html
<input type="text" v-model="name" minlength="3">
```

### Add validation rule

Adding custom validation rules are simple! Just call `VueValidator.addRule` function and provide rule name and validate function.

```javascript
/*
 * the validate function has three arguments:
 * value: the value user filled
 * input: the form control element
 * param: the attribute value of using this rule
 */
VueValidator.addRule('myrule', function(value, input, param) {
    return {
        valid: false,
        msg: 'some error message'
    };
});
```

To use your customized rule, add the rule name as input's attribute:

```html
<input type="text" myrule="some param" v-model="xx">
```

### Async validation

To add an async validation rule, return a `Promise` in the validate function.

```javascript
VueValidator.addRule('myrule', function(value, input, param) {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            resolve({
                valid: false,
                msg: 'some error message'
            });
        }, 1000);
    });
});
```

If you have any async rules in a form, the `$valid` will be a `Promise` instead of a boolean value. You must wait for that `Promise` to check the form's validality:

```javascript
this.registerForm.$valid.then(function(valid) {
    if (valid) {
        // post data to server
    } else {
        // do something else
    }
});
```


## Todo

- [ ] Date format validation
- [ ] Documentation
- [ ] Unit testing

## Contribution

Pull requests are welcome.

