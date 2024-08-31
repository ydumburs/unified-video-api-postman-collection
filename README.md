Overview
======================
This Postman collection is designed for the Vonage Video API. It provides a set of pre-configured authentications and requests to interact with Vonage Video API endpoints efficiently.  

REST API reference: https://developer.vonage.com/en/api/video

How It Works
======================
## **Using Postman Variables**  
In this collection, items enclosed in `{{ }}` are variables (e.g., `{{application_id}}`). You can either replace these placeholders with your own values or set them as variables in Postman. For more information about Postman Collection variable, visit: https://learning.postman.com/docs/sending-requests/variables/variables/#defining-collection-variables. 

## **Authentication**  
 you need to use Bearer Authentication with a JWT generated from your Application ID and Private Key. 

JWT Generation Options:  
1. You can create a JWT using the Vonage's onlie tool: https://developer.vonage.com/en/getting-started/concepts/authentication#using-the-vonage-api-online-tool-to-generate-a-jwt.
2. You can also create a JWT using the Vonage server SDK: https://developer.vonage.com/en/getting-started/concepts/authentication#using-the-server-sdks. For convenience, configure the endpoint using the SDK and use the `Generate a JWT` request at the top of the Collection. This request includes a Postman post-request script that automatically stores the returned JWT in a Collection variable upon completion.
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
3. In addition to those options, there are several other ways to generate JWTs. For more information, please refer to the  https://developer.vonage.com/en/getting-started/concepts/authentication#generating-jwts

How to Use
======================
1. **Create a Vonage Account**  
   If you don’t have a Vonage account, sign up: https://www.vonage.co.uk/communications-apis/ to start building with free credit.
2. **Create an Application for Video**
   Create an application at Vonage dashboard https://dashboard.nexmo.com/applications and enable `Video` option.
4. **Install Postman**  
   Download the Postman desktop app: https://www.postman.com/downloads/. You can also use the Web version: https://blog.postman.com/announcing-postman-for-the-web-now-in-open-beta/.
5. **Import the Collection**  
   Import the `Vonage Video API.postman_collection.json` into Postman. For guidance, check out: https://learning.postman.com/docs/getting-started/importing-and-exporting-data/#importing-postman-data.
6. **Add JWT**  
   You need to generate a JWT for all the REST API request for Vonage's Video API. For a JWT generation, please see ????????. Save your JWT in the `jwt` Collection variable or replace `{{jwt}}` in the Auth tab with your token.
7. **Configure Collection variables (optional)**  
   Add your IDs as needed to the Collection variables and save your changes. Refer to: https://community.postman.com/t/how-do-i-set-a-collection-variable/5466 for instructions.
8. **Send Requests**  
   Modify the request body as needed and click the `Send` button.
