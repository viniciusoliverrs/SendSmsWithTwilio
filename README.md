<p>
    Create a table in Google Excel with the following columns: phone number, message body, status. 
</p>
<h2>
    Add a script using following way: Tools > Script editor<br>
    Add the following code below to the table:
</h2>
<pre>
// Spreadsheet column names mapped to 0-based index numbers. 
var PHONE_NUMBER = 0;
var MESSAGE = 1;
var STATUS = 2;

function onOpen() {
    var ui = SpreadsheetApp.getUi();
    ui.createMenu('Send SMS').addItem('Send to all', 'sendSmsToAll').addToUi();
};

function sendSmsToAll() {
    var sheet = SpreadsheetApp.getActiveSheet();
    var rows = sheet.getDataRange().getValues();

    // The shift method removes the first row and saves it into headers.
    var headers = rows.shift();

    // Try to send an SMS to every row and save their status. 
    rows.forEach(function(row) {
        row[STATUS] = sendSms(row[PHONE_NUMBER], row[MESSAGE]);
    });

    // Write the entire data back into the sheet. 
    sheet.getRange(2, 1, rows.length, headers.length).setValues(rows);
}

function sendSms(phoneNumber, message) {
    try {
        var response = UrlFetchApp.fetch("http://yourdomain.com.br/SendSmsWithTwilio/sendSms.php?message=" + message + "&telephone=" + phoneNumber);
        return response.getResponseCode() == 200 ? "OK" : "ERRO"
    } catch (e) {
        return "ERRO";
    }
}
</pre>
