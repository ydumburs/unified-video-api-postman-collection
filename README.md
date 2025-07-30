Overview
======================
This Postman collection is designed for the Vonage Unified Video API. It provides a set of pre-configured authentications and requests to interact with Vonage Video API endpoints efficiently.  

REST API reference: https://developer.vonage.com/en/api/video

How to Use
======================
1. **Create a Vonage Account**  
   If you don’t have a Vonage account, sign up: https://www.vonage.co.uk/communications-apis/ to start building with free credit.
2. **Create an Application for Video**
   Create an application at Vonage dashboard https://dashboard.nexmo.com/applications and enable `Video` capability.
4. **Install Postman**  
   Download the Postman desktop app: https://www.postman.com/downloads/. You can also use the Web version: https://blog.postman.com/announcing-postman-for-the-web-now-in-open-beta/.
5. **Import the Collection**  
   Import the `Unified Video API.postman_collection.json` into Postman. For guidance, check out: https://learning.postman.com/docs/getting-started/importing-and-exporting-data/#importing-postman-data.
6. **Configure Collection variables**  
   Click on the `Unified Video API` in the left-hand navigation tree, then select `Variables` tab in the center pane. In the `Current value` section, set your application_id, private_key, and jti. 
7. **Generate a JWT**  
   Click on the `Generate a JWT` folder in the left-hand navigation tree, then select `Postman script`. In the center pane, click `Send`. This will run the script and generate a JWT. For more information on JWT generation, please see https://github.com/ydumburs/unified-video-api-postman-collection/blob/main/README.md#authentication. 
8. **Send Requests**  
   Modify the request body as needed and click the `Send` button.

How It Works
======================
## **Using Postman Variables**  
In this collection, items enclosed in `{{ }}` are variables (e.g., `{{application_id}}`). You can either replace these placeholders with your own values or set them as variables in Postman. For more information about Postman Collection variable, visit: https://learning.postman.com/docs/sending-requests/variables/variables/#defining-collection-variables. 

## **Authentication**  
You need to use Bearer Authentication with a JWT generated from your Application ID and Private Key. The JWT is generated using your Application ID and Private key.  
  
You can generate a JWT using one of the following methods: 
1. **Vonage's onlie tool**  
   Use the tool https://developer.vonage.com/en/jwt to generate your JWT.
2. **Server-Side**  
   Use the Vonage Server SDK: https://developer.vonage.com/en/getting-started/concepts/authentication#using-the-server-sdks. For convenience, you can configure the endpoint as shown in this Node.js script https://github.com/ydumburs/vonage-node-generate-jwt. Then, use the `Server-side` request in `Generate a JWT` folder at the top of the Collection. This request includes a Postman post-request script that automatically stores the returned JWT in a Collection variable `jwt` upon completion.
```
// Postman Post-request script
const response = pm.response.json(); 
if (response.token) {
    pm.collectionVariables.set("jwt", response.token); 
    console.log("JWT has been set to collection variable:", response.token);
} else {
    console.error("JWT token not found in the response");
}
```
3. **Vonage Cli**  
   Use the Vonage Cli https://developer.vonage.com/en/getting-started/concepts/authentication#using-the-vonage-cli-to-generate-jwts
4. **Postman Pre-request Script**
   Set your `Application ID`, `Private Key`, and `JTI` in a Collection variable, then use the `Postman script` in the `Generate a JWT` folder at the top of the Collection. This script generates a JWT from your credentials and encodes using the `jsrsasign` library https://github.com/kjur/jsrsasign/blob/master/jsrsasign-all-min.js. Then it sets a JWT as the Collection variable `jwt`, which is valid for 5 minutes. 
```
// Postman Pre-request script
/**
 * Postman Pre-request Script
 * Author: Yukari Dumburs (Created: 17 Jul 2024, Updated: 1 Sep 2024)
 */

// Load jsrsasign library safely
var navigator = {};
var window = {};
eval(pm.collectionVariables.get("jsrsasign"));　// https://github.com/kjur/jsrsasign/blob/master/jsrsasign-all-min.js

// Fetch required variables
var applicationId = pm.collectionVariables.get('application_id');
var privateKey = pm.collectionVariables.get('private_key');
var jti = pm.collectionVariables.get('jti');
if (!applicationId || !privateKey || !jti) {
    throw new Error("Application ID or Private Key or jti is missing!");
}
var currentTimestamp = Math.floor(Date.now() / 1000);
var expiryTimestamp = currentTimestamp + 300; // 5 minutes

// Create JWT Header and Payload
var header = {
    "alg": "RS256",
    "typ": "JWT"
};
var payload = {
    'iat': currentTimestamp,
    'exp': expiryTimestamp,
    'application_id': applicationId,
    'jti': jti
};

// Convert header and payload to JSON strings
var sHeader = JSON.stringify(header);
var sPayload = JSON.stringify(payload);

// Generate JWT using jsrsasign
try {
    var sJWT = KJUR.jws.JWS.sign(header.alg, sHeader, sPayload, privateKey);
    pm.collectionVariables.set('jwt', sJWT);
} catch (e) {
    throw new Error("Failed to generate JWT: " + e.message);
}
```
