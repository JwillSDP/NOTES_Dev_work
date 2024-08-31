You've got the Cloud Function to create a user document in Firestore. Now, let's add the functionality to also append the new member's information to a Google Sheet! 

Here's how you can modify your `createUserDocument` function:

```typescript
// functions/src/index.ts

import * as functions from 'firebase-functions';
import * as admin from 'firebase-admin';
import { UserRecord } from 'firebase-admin/auth';
import { google } from 'googleapis';

admin.initializeApp();
const db = admin.firestore();

// **1. Authorize Google Sheets API**
const authClient = await admin.credential.getApplicationDefault(); 
const sheets = google.sheets({ version: 'v4', auth: authClient }); 

exports.createUserDocument = functions.auth.user().onCreate(async (user: UserRecord) => {
   try {
      // ... (Existing code to create user document in Firestore) 

      // **2. Get spreadsheet ID (replace with your actual ID)**
      const spreadsheetId = 'YOUR_SPREADSHEET_ID';

      // **3. Prepare data for the new row**
      const newRow = [user.displayName, user.email, user.uid];

      // **4. Append the new row to the sheet**
      await sheets.spreadsheets.values.append({
         spreadsheetId,
         range: 'Sheet1!A:C', // Assuming you want to add data to columns A, B, and C
         valueInputOption: 'USER_ENTERED', 
         requestBody: {
            values: [newRow], // Add the new row data
         },
      });

      console.log('User data added to Google Sheet!');
   } catch (error) {
      console.error('Error adding user data to Google Sheet:', error);
   }
});
```

**Explanation of Changes:**

1. **Authorize Google Sheets API:**
   - We use `admin.credential.getApplicationDefault()` to get default application credentials. This assumes your Cloud Function has the necessary permissions to access Google Sheets.
   - We create a Google Sheets client using `google.sheets({ version: 'v4', auth: authClient });`.

2. **Spreadsheet ID:**
   - Replace `'YOUR_SPREADSHEET_ID'` with the actual ID of your Google Sheet. You can find the spreadsheet ID in the URL of your Google Sheet.

3. **Prepare Row Data:**
   - We create an array `newRow` containing the user's display name, email, and UID.

4. **Append Row:**
   - We use the `sheets.spreadsheets.values.append` method to add the new row to your sheet. 
   - `range: 'Sheet1!A:C'` specifies the range where the data should be added (columns A, B, and C on "Sheet1").
   - `valueInputOption: 'USER_ENTERED'`  tells Google Sheets to treat the data as if it was entered by a user (useful for dates and other special formats).

**Important Considerations:**

- **Google Sheet Permissions:** Ensure that the service account associated with your Cloud Function has edit access to your Google Sheet. You can manage permissions in the Google Sheets sharing settings.
- **Error Handling:**  Include robust error handling in your Cloud Function to catch and log any issues that might occur during the Google Sheets API call. 
- **Column Headers (Optional):** If you don't have column headers in your spreadsheet, you can add them manually or modify the `newRow` array to include headers in the first row.

**Remember:**  This code uses the Google Sheets API v4. If you need to use a different API version, update the `version` property accordingly.

Now, whenever a new user is created, your Cloud Function will create a user document in Firestore *and* append their information to your Google Sheet! 

Let me know if you have any more questions or if you'd like help with any other Firebase or Google API integrations! 
