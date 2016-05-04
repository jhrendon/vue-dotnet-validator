#Vue dotnet validator
A vuejs validator for .NET forms.

## Summary
This package makes it possible to use .NET model validation using vue.js instead of the default jQuery validator way that microsoft dictates.
The idea is that you use this on your server rendered HTML forms which include the data-attributes that are generated by C#'s razor template engine.


##Installation
Warning: This plugin is not yet live!
`npm install --save vue-dotnet-validator`


##Usage

Using this library requires changes on two places in your application, JavaScript and your razor cshtml templates.

###JavaScript
This registers the vue components so that vue.js knows what to activate. 
```
var vueDotnetValidator from 'vue-dotnet-validator'

Vue.component('vue-dotnet-validator', vueDotnetValidator.validator; 
Vue.component('vue-dotnet-validator-group', vueDotnetValidator.validatorGroup; 

```


###Cshtml
The following code should be added to your cshtml forms. This makes sure that the validator logic is activated and adds the required references to DOM-nodes.
```
<vue-dotnet-validator-group inline-template>
    <form asp-controller="Account" asp-action="Register" method="post" v-on:submit="validate">
        <validator inline-template>
           <span asp-validation-for="LastName" v-el:message></span>
           <input type="text" asp-for="LastName" v-el:field v-model="value" />
        </validator>
        <button type="submit">Register</button>
    </form>
</vue-dotnet-validator-group>
```


##Creating custom validators
It is possible to create your own validators, below is an example of a very simple custom validator.
###JavaScript
```
var vueDotnetValidator from 'vue-dotnet-validator'

class MyCustomValidator extends vueDotnetValidator.BaseValidator {
    isValid(value) {
        return !value || value == 'Hello';
    }
}
vueDotnetValidator.validator.validators['mycustom'] = MyCustomValidator;
```

###Cshtml
To use this custom validator in your own form, make sure your custom .NET data annotation outputs a `data-val-mycustom="MESSAGE"` attribute on your `<input>` DOM node.
