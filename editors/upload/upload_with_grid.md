HTML Body:
``` html
<input type="file" id="upload_files" name="files" class="files" style="width:100%">
<div id="attachment_grid" class=" mt-3" style="font-size: small;"></div>
```
Kendo UI upload widget initialize:
```javascript
$(".files").kendoUpload({
    async: {
        saveUrl: '/api/attachment/upload',
        removeUrl: '/api/attachment/remove',
        autoUpload: true
    },
    success: function(e){
        e.files[0].name = e.response.originalname;
        var upload = $("#upload_files").data("kendoUpload");
        upload.clearAllFiles();
        kendoPopUpNotifications("success", "Successfully Uploaded File.");
        $("#attachment_grid").data("kendoGrid").dataSource.read();
    },
    upload: function(e){
        var file = $("#upload_files").data("kendoUpload").getFiles();
        e.data = {
            file_timestamp: Date.now(),
            reference_id: "{{referenceNumber}}",
            module_name: "{{moduleName}}"
        };
    }
});
```
Kendo UI grid widget initialize:
```javascript
function attachmentGrid(){
    const model = {
        id: "_id",
        fields: {
            _id: {editable: false, nullable: true},
            file_size: { type: "number", editable: false },
            posted_date: { type: "date", editable: false },
            file_name: { type: "string", editable: false }
        }
    };

    const columns = [
        { field:"posted_date", title: "Upload Date", format: "{0:dd-MM-yyyy}", filterable: false, width: "100px" },
        { field:"file_size", title: "File Size", filterable: false, width: "100px", template: `#= file_size < 1000 ? 1 : file_size/1000 # KB` },
        { field:"file_name", title: "File Name", filterable: false },
        { 
            command: 
            [
                {
                    name: 'Download',
                    text: '<span class="k-icon k-i-download m-0"></span>',
                    click: downloadFile,
                },
                { name: "destroy", text: " " }
            ], title: "Options", width: "120px"
        }
    ];

    const dataSource = new kendo.data.DataSource({
        transport: {
            read:  {
                url: "/api/attachment/query/get",
                type: "POST"
            },
            destroy: {
                url: "/api/attachment/s3/remove",
                type: "POST"
            },
            parameterMap: function(options, operation) {
                if (operation === "read"){
                    let pageSize = dataSource.pageSize(),
                    page = dataSource.page(),
                    payload = {
                        reference_id: "{{referenceNumber}}",
                        module_name: "{{moduleName}}"
                    };
                    return {
                        models: kendo.stringify(payload),
                        skip: (page - 1) * (pageSize ? pageSize : 0),
                        limit: (pageSize ? pageSize : 0),
                        sort: {}
                    };
                }
                if (operation !== "read" && options.models) {
                    return {models: kendo.stringify(options.models)};
                }
            }
        },
        batch: true,
        pageSize: 20,
        schema: {
            total: "total",
            data: "data",
            model: model
        }
    });

    $("#attachment_grid").kendoGrid({
        dataSource: dataSource,
        pageable: {
            refresh: true,
            pageSizes: true,
            buttonCount: 5
        },
        sortable: true,
        resizable: true,
        columnMenu: true,
        height: 400,
        toolbar: ['excel','search'],
        excel: {
            fileName: "attachments.xlsx",
            proxyURL: "https://demos.telerik.com/kendo-ui/service/export",
            filterable: true
        },
        noRecords: true,
        messages: {
            noRecords: "No data available on current page."
        },
        columns: columns,
        editable: "popup"
    });
}
```
Function to download file from grid:
```javascript
function downloadFile(e) {
    e.preventDefault();
    var dataItem = this.dataItem($(e.currentTarget).closest("tr"));
    window.open(`/public/attachments/${dataItem.file_name}`, '_blank');
}
```