# Kendo UI Wizard for jQuery

Kendo UI Wizard helps to create multi-step processes and guide step-by-step users in applications. The good part of wizard is that it divides any input process into multiple process. As a result you don't get to see a long list of input fields. Instead you see a nice, short, organized multi-step processes. Wizard is combination of:
1. Stepper and 
2. Form

https://demos.telerik.com/kendo-ui/wizard/index

## Kendo UI Wizard has 2 core parts:
1. Done: This is an event detector property. It is triggered as soon as Submit button is clicked. This property lets you catch the form inputs and play with the data.
2. Steps: Basically this property inlcudes the steps. Each step inlcudes a form with input fields.

# Rule of Thumb!
## When we are using kendo wizard, we will always make sure of the followings:

### 1. We will always use `resetWizard()` function to reset all input fields.
### 2. If any input field is required then always use `validationMessage`.
### 3. The `kendoWizard` object should include the following configurations:
> `pager`
```javascript
pager: true
```
> `contentPosition`
```javascript
contentPosition: "bottom"
```
> `stepper`
```javascript
stepper: {
    indicator: true,
    label: true,
    linear: true
}
```
> `actionBar`
```javascript
actionBar: true
```
> `steps.forms.orientation` If input fields are single column, then: 
```javascript
orientation: "horizontal"
```