# Clear Kendo Wizard Form fields

To clear the input fields and go bact to step 1, we can use the following code snippet.
```javascript
function resetWizard(){
    // Process 1/4: We catch the wizard
    let wizard = $("#wizard").data('kendoWizard');
    // Process 2/4: No matter which step we are in, We go back to Wizard Step 1.
    wizard.select(0);
    // Process 3/4: We loop through each steps, so that we can clear the input fields.
    wizard.steps().forEach(item=>{
        // Process 4/4: Finally we used a method called form.clear() to clear the input fields.
        item.form.clear();
    });
};
```