![file upload process](https://elasticbeanstalk-ap-southeast-1-677312808939.s3.ap-southeast-1.amazonaws.com/uploads/notes/TDRL_Banner.png)

# Alert Message Component

## 🚀 Objective:
In this document we will demonstrate how can we use kendo to show `Alert Messages` to the users. Instead of creating `Alert Message` boxes from scratch, we have created a readymade function for the developers. It uses the `kendoAlert` component to show messages. The component can also have buttons to perform any function. Below image shows the `alert message`. 

![file upload process](https://elasticbeanstalk-ap-southeast-1-677312808939.s3.ap-southeast-1.amazonaws.com/uploads/notes/alert_message_1.png)
*Figure 1: Alert Message*

The purposes of `Alert Message` box are:

- [x] Show Error messages if `jquery ajax` request fails
- [x] Get confirmation from the user for any operation (inlcuding delete operation)
- [x] Show duplicate/missing info alert
- [x] Show grid related errors

The real benefit of this function is to call it from any where within the project, as long as this function is in the kendo layout file. Another benefit is that, you do not need to create a html div element to show the dialog box. Our previous process was, 

1. you create a html div element with an ID 
2. you initiate a kendo component by catching the ID of the element

## 📖 Description:
The `Alert Message` component consists of 3 attributes as shown in the below image:

![file upload process](https://elasticbeanstalk-ap-southeast-1-677312808939.s3.ap-southeast-1.amazonaws.com/uploads/notes/alert_message_2.png)
*Figure 2: Alert Message component with title, content and action attributes*

```javascript
// Code for the above image
$("<div></div>").kendoAlert({
    title: 'Alert message',
    content: `<div class="text-center">
        <i class="k-icon k-i-exclamation-circle mb-3" style="font-size: 40px"></i> 
        <br> 
        <span class="mb-3">
            This is an alert box for demonstration purpose only.
        </span> 
        <br> 
    </div>`,
    actions: [{
        text: "Cancel",
        action: function(e){
            window.location.href = '/User';
        }
    },{
        text: "Submit",
        action: function(e){
            window.location.href = '/User';
        },
        primary: true
    }]
}).data("kendoAlert").open();
```

1. title: This is the header. Please keep in mind that, the width of the `title` area is half of the widget.
2. content: This is where the `message/body` is shown. Our standard is, show a icon above the message (As shown in the image: first element is icon, and second element is the `message/body`).
3. action: This is the footer where buttons are shown. This attribue is optional. If action attribute is not passed, then a `Ok` button will be show.

![file upload process](https://elasticbeanstalk-ap-southeast-1-677312808939.s3.ap-southeast-1.amazonaws.com/uploads/notes/alert_message_3.png)
*Figure 2: Alert Message component without action attribute*
```javascript
// Code for the above image
$("<div></div>").kendoAlert({
    title: 'Alert message',
    content: `<div class="text-center">
        <i class="k-icon k-i-exclamation-circle mb-3" style="font-size: 40px"></i> 
        <br> 
        <span class="mb-3">
            This is an alert box for demonstration purpose only.
        </span> 
        <br> 
    </div>`
}).data("kendoAlert").open();
```

## 👨‍💻 How to:
From now on, all you need to do is:

### Step 1: Copy the following code into your `kendo` layout file of your project. *(Make sure a function with the same name does not exists)*
```javascript    
    /**
    * This function is for showing any alert/notification/message to the user. It uses the kendoAlert component to show the message.
    * @param {array} [title=[]] - 'title' is the header of the dialog box. Default value is 'Alert'.
    * @param {string} content - Subject is any text and it is needed when sending plain/html coded email.
    * @param {object[]} [action=[]] - 'action' represents the action buttons for the dialog box. Default value is empty array '[]', which means there will be no button.
    */
    function kDialog(title = 'Alert', content, action = []) {
        /* action Parameter format
            action: [{
                text: "OK", //Text for the button
                action: //function that will execute if the button is clicked
                function(e){ 
                    window.location.href = '/User';
                },
                primary: true //If this is a primary button. If so, then this button will act as primary button. If not primary, then the button will act as secondary button.
            }];
        */
       /* The content can have any html elements. In the following line, we've set a default content for your reference. */
        if (!content) content = `<div>This is an alert message. You can enter your message here.</div>`;
        /* The following lines actually include the code for the dialog box*/
        $("<div></div>").kendoAlert({
            title: title,
            content: content,
            actions: action
        }).data("kendoAlert").open();
    }
```

### Step 2: Call the function from anywhere in your project using any one of the following 2 approaches:
#### 👉 The alert will have title, content and one action button to run a function
```javascript
let title = 'Email Sent',
content = `<div class="text-center">
    <i class="k-icon k-i-exclamation-circle mb-3" style="font-size: 40px"></i> 
    <br> 
    <span class="mb-3">
        Password is sent to the email address.
    </span> 
</div>`,
action = [{
    text: "OK",
    action: function(e){
        window.location.href = '/User';
    },
    primary: true
}];
kDialog(title, content, action);
```
#### 👉 The alert will have title and content but one action button
```javascript
let title = 'Invalid No.',
content = `<div class="text-center">
    <i class="k-icon k-i-exclamation-circle mb-3" style="font-size: 40px"></i> 
    <br> 
    <span class="mb-3">
        Please enter valid run sheet number.
    </span> 
</div>`;
kDialog(title, content);
```
___
## [🌐](https://thedotred.com/) | [💼](https://bd.linkedin.com/company/thedotred) | [🔗](https://www.instagram.com/thedotred/)