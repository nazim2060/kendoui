# Kendo Grid with Dropdown

## Dropdown with static data
Code snippet for dropdown with static data. When we are using dropdown with static data, we must make sure the each static data object has `text` and `value` keys as shown below. 
```javascript
let userTypeData = [
    {'text':'Super Admin', 'value':'1'},
    {'text':'Admin', 'value':'2'},
    {'text':'Vendor', 'value':'3'},
];
```
Referencing the static data in the `user_type` column of the grid. We must be careful with the model. If you look carefully, the `user_type` has a `field` property not `type` property. This is because user_type requires user to select item from a dropdown, not to enter value manually. 
```javascript
function userGrid(){
    let model = 
    {
        id: "_id",
        fields: {
            _id: { editable: false, nullable: true },
            full_name: { type: "string", validation: { required: true} },
            email_id: { type: "string", validation: { required: true} },
            user_password: { type: "string", validation: { required: true} },
            user_type: { field: "user_type", validation: { required: true} },
            isDeleted: { type: "boolean" }
        }
    };

    let column = 
    [
        { field:"full_name", title: "Name"},
        { field:"email_id", title: "User email"},
        { field:"user_password", title: "Passowrd"},
        { field:"user_type.text", title: "User Type", values: userTypeData},
        { command: 
            [
                { name: "edit", text: { edit: " ", update: "Update", cancel: "Cancel" } },
                { name: "destroy", text: " " }
            ], title: "Actions", width: "120px"
        }
    ];

    let dataSource = new kendo.data.DataSource({
        transport: {
            read:  {
                url: "/api/user/get",
                dataType: "json"
            },
            update: {
                url: "/api/user/update",
                type: "POST",
                dataType: "json"
            },
            destroy: {
                url: "/api/user/delete",
                type: "POST",
                dataType: "json"
            },
            create: {
                url: "/api/user/create",
                type: "POST",
                dataType: "json"
            },
            parameterMap: function(options, operation) {
                if (operation !== "read" && options.models) {
                    return {models: kendo.stringify(options.models)};
                }
            }
        },
        batch: true,
        pageSize: 20,
        schema: {
            model: model
        },
        error: function(e) {
            var xhr = e.xhr;
            kendo.alert(e.xhr.responseText);
        }
    });

    $("#grid").kendoGrid({
        dataSource: dataSource,
        pageable: {
            pageSizes: true,
            buttonCount: 5,
            refresh: true
        },
        mobile: true,
        sortable: true,
        reorderable: true,
        columnMenu: true,
        height: 550,
        toolbar: [
            "create",
            "excel",
            {
                template: `<span class="k-spacer"></span>`
            },
            "search"
        ],
        noRecords: true,
        messages: {
            noRecords: "No data available on current page."
        },
        columns: column,
        editable: "popup"
    });
};
```
## Dropdown with remote data
Sometimes we want to fetch data from the database and populate the dropdown. In such cases, we need to create a JavaScript function for the dropdown, and assign the function to the column. The code snippet for the JavaScript function is as follows:
```javascript
function userTypeEditor(container, options) {
    let input = $(`<input id="${options.field}" name="${options.field}" style="width: 100%;" required />`);
    input.appendTo(container)
    input.kendoDropDownList({
        autoClose: true,
        filter: "contains",
        dataTextField: 'type_name',
        dataValueField: '_id',
        noDataTemplate: "No user type found",
        dataSource: {
            type: "json",
            transport: {
                read: {
                    url: "/api/user/type/get"
                }
            }
        }
    });
}
```
Once the JavaScript function is created, then assign the funtion to the `user_type` column as shown below.
```javascript
function userGrid(){
    let model = 
    {
        id: "_id",
        fields: {
            _id: { editable: false, nullable: true },
            full_name: { type: "string", validation: { required: true} },
            email_id: { type: "string", validation: { required: true} },
            user_password: { type: "string", validation: { required: true} },
            user_type: { field: "object", validation: { required: true} },
            isDeleted: { type: "boolean" }
        }
    };

    let column = 
    [
        { field:"full_name", title: "Name"},
        { field:"email_id", title: "User email"},
        { field:"user_password", title: "Passowrd"},
        { field:"user_type.text", title: "User Type", editor: userTypeEditor},
        { command: 
            [
                { name: "edit", text: { edit: " ", update: "Update", cancel: "Cancel" } },
                { name: "destroy", text: " " }
            ], title: "Options", width: "120px"
        }
    ];

    let dataSource = new kendo.data.DataSource({
        transport: {
            read:  {
                url: "/api/user/get",
                dataType: "json"
            },
            update: {
                url: "/api/user/update",
                type: "POST",
                dataType: "json"
            },
            destroy: {
                url: "/api/user/delete",
                type: "POST",
                dataType: "json"
            },
            create: {
                url: "/api/user/create",
                type: "POST",
                dataType: "json"
            },
            parameterMap: function(options, operation) {
                if (operation !== "read" && options.models) {
                    return {models: kendo.stringify(options.models)};
                }
            }
        },
        batch: true,
        pageSize: 20,
        schema: {
            model: model
        },
        error: function(e) {
            var xhr = e.xhr;
            kendo.alert(e.xhr.responseText);
        }
    });

    $("#grid").kendoGrid({
        dataSource: dataSource,
        pageable: {
            pageSizes: true,
            buttonCount: 5,
            refresh: true
        },
        mobile: true,
        sortable: true,
        reorderable: true,
        columnMenu: true,
        height: 550,
        toolbar: [
            "create",
            "excel",
            {
                template: `<span class="k-spacer"></span>`
            },
            "search"
        ],
        noRecords: true,
        messages: {
            noRecords: "No data available on current page."
        },
        columns: column,
        editable: "popup"
    });
};
```