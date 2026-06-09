# Google Sheets Pre-Order Collation Guide

Since **The Quiet Crumbs** is a micro-batch home bakery, you can collect pre-order interest directly into a **Google Sheet** for free, without paying for third-party SaaS form providers.

Here is how to connect the registration form in `index.html` to a Google Sheet using **Google Apps Script** (takes about 3 minutes).

---

## Step 1: Create Your Google Sheet
1. Open [Google Sheets](https://sheets.google.com) and create a new blank spreadsheet.
2. Name it something like `The Quiet Crumbs - Pre-Order Interest`.
3. Set the first row headers to:
   - Column A: `Timestamp`
   - Column B: `Name`
   - Column C: `Email`
   - Column D: `Country Code`
   - Column E: `Phone`
   - Column F: `Flavours of Interest`

---

## Step 2: Open the Apps Script Editor
1. In the Google Sheets menu, click **Extensions** -> **Apps Script**.
2. Delete any code inside the default `Code.gs` file.
3. Paste the following Apps Script code into the editor:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    var date = new Date();
    
    // Append a new row with date, name, email, country code, phone, and flavours
    sheet.appendRow([
      date,
      data.name || "",
      data.email || "",
      data.countryCode || "",
      data.phone || "",
      data.flavours || ""
    ]);
    
    return ContentService.createTextOutput(JSON.stringify({ "status": "success" }))
      .setMimeType(ContentService.MimeType.JSON)
      .setHeader('Access-Control-Allow-Origin', '*');
  } catch (error) {
    return ContentService.createTextOutput(JSON.stringify({ "status": "error", "message": error.toString() }))
      .setMimeType(ContentService.MimeType.JSON)
      .setHeader('Access-Control-Allow-Origin', '*');
  }
}

// Handle CORS Preflight OPTIONS requests
function doOptions(e) {
  return ContentService.createTextOutput("")
    .setMimeType(ContentService.MimeType.TEXT)
    .setHeader('Access-Control-Allow-Origin', '*')
    .setHeader('Access-Control-Allow-Methods', 'POST, GET, OPTIONS')
    .setHeader('Access-Control-Allow-Headers', 'Content-Type');
}
```

4. Click the **Save** icon (disk symbol) or press `Cmd+S` / `Ctrl+S`.

---

## Step 3: Deploy as a Web App
1. Click the **Deploy** button at the top-right and select **New deployment**.
2. Click the gear icon next to "Select type" and choose **Web app**.
3. Fill out the fields:
   - **Description**: `Quiet Crumbs Pre-Order Form API`
   - **Execute as**: `Me (your-email@gmail.com)`
   - **Who has access**: **`Anyone`** *(This is critical; it must be 'Anyone' so the website can submit data to it).*
4. Click **Deploy**.
5. Google will ask you to authorize permissions. Click **Authorize Access**, log into your Google Account, click **Advanced**, and then click **Go to Untitled project (unsafe)** to grant permissions.
6. Once deployed, copy the **Web app URL** (it looks like `https://script.google.com/macros/s/.../exec`).

---

## Step 4: Add the URL to index.html
1. Open [index.html](file:///Users/hanken/Work/Codes/Development/quietcrumbs/index.html).
2. Locate the JavaScript section at the bottom.
3. Paste your Web App URL into the `FORM_ENDPOINT` variable:

```javascript
// Google Sheets Web App URL (or other POST endpoint)
const FORM_ENDPOINT = "YOUR_PASTED_WEB_APP_URL_HERE";
```

4. Save the file. Your pre-orders will now flow directly into Google Sheets in real-time!
