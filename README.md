# WebAppGooglePhotos

1. Set up a Google Cloud Project
- Go to the Google Cloud Console.
- Create a new project.
- Once the project is created, navigate to the API & Services dashboard.
  
2. Enable Google APIs
- In your Google Cloud project, go to API & Services > Library.
- Enable the following APIs:
  - Google Photos Library API
  - Google Maps JavaScript API
  - Google Identity Platform (for OAuth)
    
3. Set up OAuth Credentials
- Navigate to APIs & Services > Credentials.
- Click on Create Credentials and select OAuth 2.0 Client IDs.
- Set the application type to Web Application.
- In Authorized redirect URIs, add the URL where Google will redirect users after login (e.g., http://localhost:3000/callback for local development).
- This will generate a Client ID and a Client Secret, which you will use for your authentication flow.

4. Add Google Sign-In to Your Web App
- Include the Google Sign-In JavaScript library in your HTML:
<script src="https://accounts.google.com/gsi/client" async defer></script>

- Create a button for Google Sign-In:
<div id="g_id_onload"
     data-client_id="YOUR_GOOGLE_CLIENT_ID"
     data-callback="handleCredentialResponse"
     data-auto_select="true">
</div>
<div class="g_id_signin" data-type="standard"></div>

5. Handle the User Authentication Response
- In your JavaScript file, handle the login response:
  
function handleCredentialResponse(response) {
    console.log("Encoded JWT ID token: " + response.credential);
    // Send the JWT token to your backend for validation and use
}

- Once the user logs in, you'll receive an ID token (JWT) that you can verify in your backend to ensure authentication.

6. Backend Token Verification (Optional)
- If you have a backend (e.g., Node.js), you can verify the token like this:

const {OAuth2Client} = require('google-auth-library');
const client = new OAuth2Client(CLIENT_ID);

async function verify(token) {
  const ticket = await client.verifyIdToken({
      idToken: token,
      audience: CLIENT_ID,
  });
  const payload = ticket.getPayload();
  const userid = payload['sub'];
  console.log(userid);
}

7. Requesting Permissions for Google Photos and Maps
After the user logs in, you need to request access to their Google Photos and Maps data. This is done by adding scopes to your OAuth request:

- For Google Photos: https://www.googleapis.com/auth/photoslibrary.readonly
- For Google Maps: https://www.googleapis.com/auth/maps
Update your sign-in code to include these scopes.

