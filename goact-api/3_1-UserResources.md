# User account related resources
 
## 1. GET /mint/api/v1/user/:access_token

Return the current user profile data.

### Authentication

Request must be authenticated by Application specific token.

### Example Request

```sh
curl -i -H "Content-Type: application/json" -H "Authorization: ApplicationToken 1YotnFZsEjr1zCsicMWpAAFSa" -X GET  https://test.goact.co/mint/api/v1/user/dbd4bc88-7f44-4cd7-b9f6-06db922e36c2
```
### Example Response

```javascript
{ 
  "username" : "kc@goact.com.au",
  "firstname" : "Kate",
  "lastname" : "Smith",
  "date_of_birth" : "1981-03-05",
  "sex" : "male",
  "weight" : 65.5,
  "height" : 169.0, 
  "created" : 1371472503.646541,
  "updated" : 1371492826.623422
}
```

If the field value is not set, it is **null**.

### Fields

Fields that belong to the user account

Field | Description
---------|--------
id | Unique and permanent user id.
username | User's email address. **Must not be null**
firstname | User's firstname.
lastname | User's lastname.
date_of_birth | *Optional* User's date of birth.
sex | *Optional* Biological sex. Choises are "M", "F", and null.
weight | *Optional* Weight in kilograms
height | *Optional* Height in centimeters 
created | Timestamp of user creation
updated | Timestamp of user profile update


## 2. PUT /mint/api/v1/user/:access_token

Update all or some of the fields in user profile.

### Authentication

Request must be authenticated by Application specific token.


### Fields

Fields that can update to the user account.

Field | Description
---------|-------- 
firstname | *Optional* User's firstname.
lastname | *Optional* User's lastname.
date_of_birth | *Optional* User's date of birth(YYYY-MM-DD).
sex | *Optional* Biological sex. Choises are "male", "female", and null.
weight | *Optional* Weight in kilograms
height | *Optional* Height in centimeters  
password | *Optional* new password to change. Minimum length is 6. The native app should confirmed that new password is correct by double checking.     
deviceid | *Optional* Device id for native app to send notification   
FCMPushToken | *Optional* FCM Push Token  
APNSPushToken | *Optional* APNS Push Token   
pushPermissionGranted | *Optional* Push Permission Granted   

 
### Request

The data to update is sent in request body in JSON format, as in the GET
request. id, username, created, and updated fields are ignored, if present.

Fields can be removed (cleared) by setting a field value to null in the request.

```sh
curl -i -H "Content-Type: application/json" -H "Authorization: ApplicationToken 1YotnFZsEjr1zCsicMWpAAFSa" -X PUT -d '{"firstname":"Kate", "lastname":"Smith", "date_of_birth":"1981-03-05","sex":"male", "weight" : 65.5, "height" : 169.0, "password": "newpaswd!23", "deviceid" : "bk3RNwTe3H0:CI2k_HHwgIpoDKCIZvvDMExUdFQ3P" }' https://test.goact.co/mint/api/v1/user/dbd4bc88-7f44-4cd7-b9f6-06db922e36c2
```

### Response

Fields that requested will be returned with.

Returns the updated user profile, see GET request.





## 3. POST /mint/api/v1/user/humanapi/:access_token

Update Humanapi api fields to access and fetch Health device data from Humanapi.
This api must be call whenever user add new device.

### Authentication

Request must be authenticated by Application specific token.

### Example Request

```sh
curl -i -H "Content-Type: application/json" -H "Authorization: ApplicationToken 1YotnFZsEjr1zCsicMWpAAFSa" -X POST -d '{"accessType" : "update", "humanId":"51b946bac50f459051f85713d716cb07", "accessToken":"LWtZp5evX5KcEvvi_xvdISRkLgY=bw7s2IdY42fb85ea06c98a63d44c7a41fea208a8540a661f8ad9328c7915527c8c9d402234d1bb5c76efd98c126d098f16adf1aa5004fb865245112d24f4e3f3ecdb3d6874efba9c1e9954ceb2eaf85f531f624505cbdd0667e7af463a82de224004bb6475a6fa13a4b6308e702f592d5a6b104e", "publicToken" : "9fa02df6631b81bf3f2405a642592b8b" }'  https://test.goact.co/mint/api/v1/user/humanapi/dbd4bc88-7f44-4cd7-b9f6-06db922e36c2
```
### Example Response 1 if user has already conntected with Humanapi

```javascript
{ 
  "user" : 1231,
  "humanId" : "51b946bac50f459051f85713d716cb07",
  "accessToken":"LWtZp5evX5KcEvvi_xvdISRkLgY=bw7s2IdY42fb85ea06c98a63d44c7a41fea208a8540a661f8ad9328c7915527c8c9d402234d1bb5c76efd98c126d098f16adf1aa5004fb865245112d24f4e3f3ecdb3d6874efba9c1e9954ceb2eaf85f531f624505cbdd0667e7af463a82de224004bb6475a6fa13a4b6308e702f592d5a6b104e", 
  "publicToken" : "9fa02df6631b81bf3f2405a642592b8b"
}
```

### Example Response 2 if user hasn't conntected with Humanapi yet.

```javascript
{ 
  "user" : 1231
}
```

If the field value is not set, it is **null**.

### Fields

Fields that belong to the user account

Field | Description
---------|-------- 
accessType  | 'get' or 'update' **Must not be null**.
user        | It should be matched with 'client_user_id' when communicate with Humanapi. This field also returns in sign in api('/mint/api/v1/auth/authorize').
humanId     | Unique and Humanapi user id for this user. **Must not be null** if accessType is 'update'.
publicToken | Humanapi public token for this user. **Must not be null** if accessType is 'update'.
accessToken | Humanapi access token for this user. **Must not be null** if accessType is 'update'.


### Rule for client_user_id 
When communicating with Hunman api, they will ask 'client_user_id'.
Since we have several server instance we need to identify each user by adding web domain.
To do this we should provide it as 'sahmri.goact.co-' + user. eg, sahmri.goact.co-1157. 






