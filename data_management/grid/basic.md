# Basic Kendo Grid Component

## Basic Grid structure [**popup edit mode, error handler**] 
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
            isDeleted: { type: "boolean" }
        }
    };

    let column = 
    [
        { field:"full_name", title: "Name"},
        { field:"email_id", title: "User email"},
        { field:"user_password", title: "Passowrd"},
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