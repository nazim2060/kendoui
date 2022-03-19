# Basic Kendo Wizard Component

## Basic Wizard structure [**custom submit handler**]
First we need to create an `div` element with an id of `wizard`, which we will use to initialize the `wizard`. You can place this `div` element wherever you want. You can even put the element in a kendo window. In our case, we will be using window in which will show the wizard.
```html
<div id="wizard"></div>
```
Now initialize the kendo wizard using the following code snippet. In this code snippet, we enabled pager to let the user know how many pages remained. In the configuration, the `done` property catches the form input using the code `e.forms`.  
```javascript
let wizard = $("#wizard").kendoWizard({
    "pager": true,
    "done": function (e) {
        e.originalEvent.preventDefault();
        let payload = {};
        // We have two steps in our wizard, so `e.forms` will have two array of objects. Within these objects,
        // there will be input fields and values. 
        e.forms.forEach(item=>{
            Object.keys(item._model.toJSON()).forEach(key=>{
                payload[key] = item._model[key];
            });
        });
        // Now we have proper json object in the payload, curated from the wizard.
        console.log(payload);
    },
    "contentPosition": "bottom",
    "stepper": {
        indicator: true,
        label: true,
        linear: true
    },
    "actionBar": true,
    "steps": [
        {
            title: "Bank Account",
            buttons: ["next"],
            form: {
                orientation: "horizontal",
                formData: {
                    bank_name: null,
                    bank_address: null,
                    branch_name: null,
                    account_number: null
                },
                items: [
                    { field: "bank_name", 
                        label: "Bank name:", 
                        validation: { 
                            required: true, 
                            validationMessage: 'This field is required'
                        } 
                    },
                    { field: "bank_address", 
                        label: "Bank Address:", 
                        validation: { 
                            required: true, 
                            validationMessage: 'This field is required'
                        } 
                    },
                    { field: "branch_name", 
                        label: "Branch Name:", 
                        validation: { 
                            required: true, 
                            validationMessage: 'This field is required'
                        } 
                    },
                    { field: "account_number", 
                        label: "Account Number:", 
                        validation: { 
                            required: true, 
                            validationMessage: 'This field is required'
                        } 
                    },
                    {
                        field: "ledger_option",
                        editor: "RadioGroup",
                        label: "Choose an option:",
                        validation: { required: true },
                        editorOptions: {
                            items: ["Create a ledger", "Link with a ledger"],
                            layout: "horizontal",
                            labelPosition: "after",
                        }
                    },
                ]
            }
        },
        {
            title: "Ledger Details",
            buttons: ["previous", "done"],
            form: {
                orientation: "horizontal",
                formData: {
                    ledger_id: ""
                },
                items: [
                    {
                        field: "ledger_id", 
                        editor: "DropDownList", 
                        label: "Select a ledger:", 
                        validation: { 
                            required: true, 
                            validationMessage: 'This field is required' 
                        },
                        editorOptions: {
                            placeholder: "Select a ledger",
                            dataTextField: "ledger_name",
                            dataValueField: "_id",
                            filter: "contains",
                            dataSource: {
                                type: "json",
                                transport: {
                                    read:  {
                                        url: "/api/accounts/ledger/query/get",
                                        type: "POST"
                                    },
                                    parameterMap: function(options, operation) {
                                        if (operation === "read") {
                                            return {models: kendo.stringify({
                                                reference_number: null
                                            })};
                                        }
                                        if (operation !== "read" && options.models) {
                                            return {models: kendo.stringify(options.models)};
                                        }
                                    }
                                }
                            }
                        }
                    }
                ]
            }
            
        },
    ]
});