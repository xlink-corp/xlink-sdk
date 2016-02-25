# Xlink Platfrom APIs Document

    With an Xlink account, user will be able to retrieve an access token to access to Xlink RESTful interface.
    User can register an Xlink account can be registered with user's mobile phone number or email.


#**At a Glance**

1. [Register with Email Address](#RegisterwithEmail)
2. [Activate via Email](#ActivateViaEmail)
3. [Register with Mobile Phone Number](#RegisterWithPhone)
4. [Send SMS Verification Code](#SMSVerificationCode)
5. [Login and Validating Access Token](#LoginAndValidating)
6. [Renew Access Token](#RenewAccessToken)
7. [Retrieve User Information](#RetrieveUserInformation)
8. [Edit User Information](#EditUserInformation)
9. [Change Password](#ChangePassword)
10. [Forgot Password](#ForgotPassword)
11. [Appendix](#Appendix)

#**Xlink APIs**

### ***About Enterprise ID:***

    Users' operations including register, verification,  reset password required valid Enterprise ID, Enterprise ID can be found on Xlink console.


### **<a name="RegisterwithEmail">1.Register With Email Address</a>**
    User can register an Xlink account using email address, an activation email will be sent to user's email, user will be able to activate the account via the url within the email.
    User activation is optional and can be disable by enterprise user within Xlink console.



**Request**

URL

    POST /v2/user_register

![](http://i.imgur.com/IYW1aIs.png)

Header

    Content-Type : "application/json"

Content

    {
        "email":"Email address",
        "nickname":"Nickname",
        "corp_id":"Enterprise ID",
        "password":"Password",
        "source":"Source",
        "local_lang":"Local language code"
    }    

**Response**

Header

    HTTP/1.1 200 OK

Content

    {
        "email":"Email address"
    }

| Variable | Required | Description
| -- | -- | --
| email | Yes | Email Address
| nickname | Yes | User Nickname，2-32 character length
| corp_id | Yes | Enterprise ID
| password | Yes | User Password，6-16 character length
| source | Yes | User Source，*see appendix
| local_lang | No | Local Language Code，default：zh-ch，*see appendix



### **<a name="ActivateViaEmail">2.Activate via Email</a>**

**Request**

URL

    POST /v2/user_email_activate

Header

    HTTP/1.1 200 OK

Content

    {
        "corp_id":"Enterprise ID",
        "verifycode":"Verification code",
        "email":"Email address"
    }

**Response**

Header

    HTTP/1.1 200 OK

Content

    无

### **<a name="RegisterWithPhone">3.Register With Mobile Phone Number</a>**


    User can register with mobile phone number, a verification code will be sent to users' phone and user activate with the valid code.
    *Regional Support: Mainland China


**Request**

URL

    POST /v2/user_register

Header

    Content-Type : "application/json"
Content

    {
    "phone":"Mobile phone number",
    "nickname":"Nickname",
    "corp_id":"Enterprise ID",
    "verifycode":"Verification code",
    "password":"User password",
    "source":"Source",
    "local_lang":"Local language code"
    }


**Response**

Header

    HTTP/1.1 200 OK

Content

    {
        "phone":"Mobile Phone Number"
    }

| Variable | Required | Description
| --- | --- | ---
| phone | Yes | Mobile Phone Number
| nickname | Yes | User Nickname，2-32 character length
| corp_id | Yes | Enterprise ID
| verifycode | Yes | Mobile Verification SMS Code
| password | Yes | User Password，6-16 character length
| source | Yes | User Source，*see appendix
| local_lang | No | Local Language Code，default：zh-ch，*see appendix




### **<a name="SMSVerificationCode">4.Send SMS Verification Code</a>**

    Call for this API to send SMS text to user's mobile phone. User input the valid verification code to register and activate account, the SMS valid session is 120 second.

**Request**

URL

    POST /v2/user_register/verifycode
Header

    "Content-Type":"application/json"

Content

    {
    "corp_id":"Enterprise ID",
    "phone":"Mobile Phone Number"
    }

**Response**

Header

    HTTP/1.1 200 OK
Content

    无



### **<a name="LoginAndValidating">5.Login And Validating Access Token</a>**


    User login with username(email or phone number) and password to access to Xlink RESTful interface. An access token and a refresh token will be validated to user.  
    The token valid for 2 hours.

![](http://i.imgur.com/UpYcWcb.png)

**Request**

URL

    POST /v2/user_auth

Header

    "Content-Type":"application/json"

Content

    {
        "corp_id":"Enterprise ID",
        "phone/email" : "Mobile phone number/email",
        "password" : "Password"
    }


| Variable | Required | Description
| --- | --- | ---
| corp_id |  Yes| Enterprise ID
| phone/email | Yes | Mobile Phone Number or Email Address（Depend on user's registration）
| password | Yes | User Password，6-16 character length


**Response**

Header

    HTTP/1.1 200 OK

Content

    {
        "user_id":"User ID",
        "access_token":"Access token",
        "refresh_token":"Refresh token",
        "expire_in":"Expired in（seconds）"
    }


| Variable | Required | Description
| --- | --- | ---
| user_id | Yes | User ID
| access_token | Yes | Xlink RESTful Interface Access Token
| refresh_token | Yes | Refresh Token, to gain a new token
| expire_in| Yes | Valid Time,in unit of seconds


### **<a name="RenewAccessToken">6.Renew Access Token</a>**


    Access Token only valid for 2 hours, developer can use this interface to refresh a new access token and a new refresh token. If the Access Token had expired before the refresh, user has to relogin.


**Request**

URL

    POST /v2/user/token/refresh
Header

    Content-Type:"application/json"
    Access-Token:"Access Token"

Content

    {
        "refresh_token":"Refresh Token"
    }


**Response**

Header

    HTTP/1.1 200 OK

Content

    {
        "access_token":"New access token",
        "refresh_token":"New refresh token",
        "expire_in":"Expired in (second)"
    }


### **<a name="RetrieveUserInformation">7.Retrieve User Information</a>**

    Use this interface to retrieve user personal information

**Request**

URL

    GET /v2/user/{user_id}

Header

    Content-Type : "application/json"
    Access-Token : "Access token"

Content

    无

**Response**

Header

    HTTP/1.1 200 OK

Content

    {
        "id" : "User ID",
        "corp_id":"Enterprise ID",
        "phone/email" : "Mobile phone number/Email address",
        "nickname" : "User nickname",
        "authorize_code":"Authorize code",
        "create_date" : "Create date",
        "status":"User status",
        "source" : "Source",
        "region_id":"User location region ID",
        "is_vaild":"If activated"
    }

| Variable | Required | Description
| --- | --- | ---
| id | Yes | User ID
| corp_id | Yes | Enterprise ID
| phone/email | Yes | Mobile Phone Number/Email Address
| nickname | Yes | User Nickname
| authorize_code | Yes | User Authorize Code
| create_date | Yes | Account Create date and time，example:2015-10-09T08:15:40.843Z
| status | Yes | User Status，*see appendix
| source | Yes | User Source，*see appendix
| region_id | Yes | User Location Regional ID
| is_vaild | Yes | If Activated

### **<a name="EditUserInformation">8.Edit User Information</a>**


    Edit user basic information, only support to change User Nickname for now.


**Request**

URL

    PUT /v2/user/{user_id}

Header

    Content-Type : "application/json"
    Access-Token : "Access Token"

Content

    {
        "nickname" : "User Nickname"
    }

**Response**

Header

    HTTP/1.1 200 OK

Content

    无


### **<a name="ChangePassword">9.Change Password</a>**

    Change Password

**Request**

URL

    PUT /v2/user/password/reset

Header

    Content-Type : "application/json"
    Access-Token : "Access token"

Content

    {
        "old_password" : "Old password",
        "new_password" : "New password"
    }

**Response**

Header

    HTTP/1.1 200 OK

Content

    无


### **<a name="ForgotPassword">10.Forgot Password</a>**


    User can reset password via email or mobile phone when password had lost/forgot.
    *Only activated user can reset password

 **Process as follows**


![](http://i.imgur.com/3FA1lhI.png)



    Xlink had provided forgot password Email Template and password Reset Page. It can be found and customize in Xlink Console, in only required to set variable in Email Template page.

| Email Variable | Description |
| --- | --- |
|%verifycode% |Verification Code |
|%corp_id% | Enterprise ID |
|%email% | Email Address |



** A. Request Password Reset**

    Request password reset process using user name.
    If user were registered by email, an URL with be sent by email to user to reset password.
    If user were registered by mobile phone number, a 6-digit verification code will send to user by SMS, user can use the code to reset password.


**Request**

URL

    POST /v2/user/password/forgot

Header

    Content-Type : "application/json"

Content

    {
        "corp_id":"Enterprise ID",
        "phone/email":"Mobile Phone Number/Email Address"
    }

**Response**

Header

    HTTP/1.1 200 OK

Content

    无


** B.Reset Password Via Verification Code**

    Send a verification code to user by email/sms(depend on registration), user can reset password with the code.


** Request **

URL

    POST /v2/user/password/foundback
Header

    Content-Type : "application/json"

Content

    {
        "corp_id":"Enterprise ID",
        "phone/email" : "Mobile phone number/Email address",
        "verifycode" : "Verification code",
        "new_password" : "New password"
    }

| Variable | Required | Description
| --- | --- | ---
| corp_id | Yes | Enterprise ID
| phone/email | Yes | Mobile Phone Number or Email Address
| verifycode | Yes | Verification Code
| new_password | Yes | New Password

**Response**

Header

    HTTP/1.1 200 OK
Content

    无


### **<a name="Appendix">11.Appendix</a>**

**User Source**

|Enumeration Value| Description |
| --- | --- |
|1 | Web|
|2 | Android App
|3 | IOS App
|4 | Wechat
|5 | QQ
|10| Other source follows Xlinks user verification source


**User Status**

|Enumeration Value| Description |
| --- | --- |
|1 | Normal
|2 | Blocked |



**Local Language Code**

|Enumeration Value| Description |
| --- | --- |
|zh-cn| Chinese（Simplified）
|en-us| English（United States）
