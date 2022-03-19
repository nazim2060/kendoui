# Dynamic Kendo Wizard Component

## Dynamic Wizard structure [**Changes step based on radio button**]

### Our objective in this case is to replace step 2 based on user input. In Step 1, we have a radio field called `ledger_option`. This radio button have two options, 1) Create a ledger 2) Link with a ledger. If user selects any one of these options, then Step 2 will change. By default Step 2 already has content which is for Link with a ledger option. We can achieve this using the code snippets.

First we need to create an `div` element with an id of `wizard`.
```html
<div id="wizard"></div>
```
Now initialize the kendo wizard using the following code snippet.
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
                            select: function(e){
                                // $(e.target[0]).val() commad will have the option selected by the user.
                                if($(e.target[0]).val() === 'Link with a ledger') {
                                    $("#wizard").data('kendoWizard').removeAt(1);
                                    $("#wizard").data('kendoWizard').insertAt(1, {
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
                                                                            parent_id: `{{{cashLedger._id}}}`,
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
                                        
                                    });
                                } else if ($(e.target[0]).val() === 'Create a ledger') {
                                    $("#wizard").data('kendoWizard').removeAt(1);
                                    $("#wizard").data('kendoWizard').insertAt(1, {
                                        title: "Create Ledger",
                                        buttons: ["previous", "done"],
                                        form: {
                                            orientation: "horizontal",
                                            formData: {
                                                // parent_id: '',
                                                ledger_code: null,
                                                ledger_name: null
                                            },
                                            items: [
                                                { field: "ledger_code", 
                                                    label: "Sub-ledger Code:", 
                                                    validation: { 
                                                        required: true, 
                                                        validationMessage: 'This field is required' 
                                                    } 
                                                },
                                                { field: "ledger_name", 
                                                    label: "Sub-ledger Name:", 
                                                    validation: { 
                                                        required: true, 
                                                        validationMessage: 'This field is required' 
                                                    } 
                                                },
                                            ]
                                        }
                                    });
                                }
                            }
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
```