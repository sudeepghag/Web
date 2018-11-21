#### Salesforce Merge Accounts URL Hacking:
https://fosnez.blogspot.com/2012/11/salesforce-merge-accounts-url-hacking.html


#### Salesforce Email URL Hack:
https://www.salesforceben.com/salesforce-email-url-hack-tutorial/


#### Salesforce URL Hacking – Tutorial
https://www.salesforceben.com/salesforce-url-hacking-tutorial/


#### Salesforce Reports URL Hack:
https://www.salesforceben.com/salesforce-reports-url-hack/

### Custom Button and Link Samples
https://help.salesforce.com/articleView?id=links_useful_custom_buttons.htm&type=5

#### Custom Link Example: Link to Documents:

- Create a folder on the Documents tab to which all users have access.
- Upload the document to that folder.
- From the Documents tab, choose the folder and click Go.
- Click View next to the document.
- Copy the document’s URL from the browser. For example, https://na1.salesforce.com/servlet/servlet.FileDownload?file=015300000000xvU.
- Use everything after the domain portion of the URL to create your custom link. Using the example in the previous step, your link would point to /servlet/servlet.FileDownload?file=015300000000xvU.

#### Custom Link Example: Link to Files in Chatter
- Upload a file to the Files tab.
- When the upload is finished, from the Upload dialog box, click Share settings.
- Click Anyone with link.
- Copy the document’s URL from the Share via Link dialog box. For example, https://na1.salesforce.com/sfc/p/D0000000JsES/a/D000000001dd/aiq8UPJ5q5i6Fs4Sz.IQLKUERsWYdbAm320cjqWnkfk=.
- Use everything after the domain portion of the URL to create your custom link. Using the example in the previous step, your link would point to /sfc/p/D0000000JsES/a/D000000001dd/aiq8UPJ5q5i6Fs4Sz.IQLKUERsWYdbAm320cjqWnkfk=.

#### Custom Link Example: Link to Reports
- Copy the ID for the type of record by which you want to filter your report (in this example, an account). To do so, view the record and copy the 15-character ID from the last part of the URL. For example, https://na1.salesforce.com/001200030012j3J.
- From the Reports tab, create the report you want by either customizing a standard report or creating a custom report.
- Filter the report by the record ID you copied. For example, “Account ID equals 001200030012j3J”.
- Run the report to verify that it contains the data you expect.
- Click Customize.
- Click Save or Save As to save the report to a public folder accessible by the appropriate users. Save doesn’t create a custom report, whereas Save As does.
- Run the report and copy the report’s URL from the browser.
- Begin creating your custom link. Set the Content Source field to URL. In the large formula text area, paste the report URL you copied. - Remember to omit the domain portion https://na1.salesforce.com.
- Add the custom link to the appropriate page layouts.
- Verify that the new custom link works correctly.

#### Displaying Alerts
- Define a button with the following attributes.
- Display Type─Detail Page Button
- Behavior─Execute JavaScript
- Content Source─OnClick JavaScript
- Use the following sample code.
`alert ("Hello {!$User.FirstName}");`
- Add the button to the appropriate page layout.

#### Getting Record IDs
- Define a button with the following attributes.
- Display Type─List Button
Note
- NOTE Select Display Checkboxes (for Multi-Record Selection) so users can select multiple records in the list before clicking the button.
- Behavior─Execute JavaScript
- Content Source─OnClick JavaScript
- Use the following sample code.
```javascript
 idArray = {!GETRECORDIDS($ObjectType.Contact)};
 alert("The Ids you have selected are: "+idArray);
```
- NOTE This example is for contacts. Change the object type for a different type of record.
- Add the button to the appropriate related list on a page layout or list view layout.

#### Create a Custom Button for Performing Mass Deletes
```javascript
{!REQUIRESCRIPT("/soap/ajax/9.0/connection.js")} 

var records = {!GETRECORDIDS( $ObjectType.Event )}; 
var taskRecords = {!GETRECORDIDS( $ObjectType.Task)}; 
records = records.concat(taskRecords); 


if (records[0] == null) { 
alert("Please select at least one record.") } 
else { 

var errors = []; 
var result = sforce.connection.deleteIds(records); 
if (result && result.length){ 
var numFailed = 0; 
var numSucceeded = 0; 
for (var i = 0; i < result.length; i++){ 
var res = result[i]; 
if (res && res.success == 'true'){ 
numSucceeded++; 
} else { 
var es = res.getArray("errors"); 
if (es.length > 0) { 
errors.push(es[0].message); 
} 
numFailed++; 
} 
} 
if (numFailed > 0){ 
alert("Failed: " + numFailed + "\nSucceeded: " + numSucceeded + " \n Due to: " + errors.join("\n")); 
} else { 
alert("Number of records deleted: " + numSucceeded); 
} 
} 
window.location.reload(); 
}
```

#### Passing Record IDs to an External System
```javascript
<script type="text/javascript">
idArray = {!GETRECORDIDS($ObjectType.Account)};
window.location.href="http://www.yourwebsitehere.com?array="+idArray;
</script>
```


#### Reopening Cases
- Display Type─List Button
- NOTE Select Display Checkboxes (for Multi-Record Selection) so users can select multiple records in the list before clicking the button.
- Behavior─Execute JavaScript
- Content Source─OnClick JavaScript
- Use the following sample code.
```javascript
{!REQUIRESCRIPT ("/soap/ajax/13.0/connection.js")} 
var records = {!GETRECORDIDS($ObjectType.Sample)}; 
var newRecords = []; 

if (records[0] == null) { 
     alert("Please select at least one row") 
} 
else {
      for (var n=0; n<records.length; n++) { 
         var c = new sforce.SObject("Case"); 
         c.id = records[n]; 
         c.Status = "New";
         newRecords.push(c); 
      } 
        result = sforce.connection.update(newRecords); 
        window.location.reload(); 
 }
```
#### International Maps
```javascript
{! 
IF(Sample.BillingCountry = "US", 
"http://maps.google.com/maps?q=​"&Sample.BillingStreet&​
"+"&Sample.BillingCity&"+​"&Sample.BillingState&"+​"&Sample.BillingCountry, 
(IF(Sample.BillingCountry = "UK", 
"http://maps.google.co.uk/maps?q=​"&Sample.BillingStreet
&​"+"&Sample.BillingCity&​"+​"&Sample.BillingCountry, 
"http://maps.google.com"))) 
}
```
