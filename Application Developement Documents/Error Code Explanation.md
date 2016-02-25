# Error Code Explanation


Xlink platform provide RESTful style interface for developers, HTTP Response Code allow developers to understand the connectivity of the interface.

Error Code Explanation：

|HTTP Response| Explanation
| --- | --- |
| HTTP/1.1 200 OK | Request success, returned correct value
| HTTP/1.1 400 Bad Request | Parameter error
| HTTP/1.1 403 Forbidden | Interface authorization error
| HTTP/1.1 404 Not Found | Cannot locate document
| HTTP/1.1 503 Service Unavailable | Server exception


**Unless HTTP Response Code is 200 and return correct data, other Response Code will be in followed format:**

    {"error":{"code":"error code","msg":"error explanation"}}


#### ***Error Code Format:{HTTP return value}+{error code}***


**Error Code Explanation: 400**



| Code | Error Code Explanation|
| --- | ---
| 4001001 | Requested variable verification failed
| 4001002 | Requested variable null.
| 4001003 | SMS verification code not found
| 4001004 | Invalid SMS verification code
| 4001005 | This mobile number has already been registered
| 4001006 | This email address has already been registered
| 4001007 | Wrong password
| 4001008 | Account illegal
| 4001009 | Enterprise member account status illegal
| 4001010 | Refresh-Token illegal
| 4001011 | Unknown member type
| 4001012 | Only Administrator can send invitation
| 4001013 | Unable to edit other member's information
| 4001014 | Unable to delete your own account
| 4001015 | Unknown product connectivity type
| 4001016 | Unable to delete published product
| 4001017 | Firmware version exist
| 4001018 | Data point unknown data type
| 4001019 | Data point index has exist
| 4001020 | Unable to delete published data point
| 4001021 | MAC address has already exist of this product
| 4001022 | Unable to delete activated device
| 4001023 | Extension properties Key were default property
| 4001024 | Device extensions reached limit
| 4001025 | Unable to add existed extension properties
| 4001026 | Unable to update not existed extension properties
| 4001027 | Variable name illegal
| 4001028 | Email verification code does not exist
| 4001029 | Wrong email verification code
| 4001030 | User account status illegal
| 4001031 | User phone number unverified
| 4001032 | User email address unverified
| 4001033 | Device has already been subscribed
| 4001034 | User did not subscribed the device
| 4001035 | Auto update task already exist
| 4001036 | Updating status unknown
| 4001037 | Initial version updating task already exist
| 4001038 | Device activation failed
| 4001039 | Device verification failed
| 4001041 | Wrong Subscribed device verification code
| 4001042 | Authentic name exist
| 4001043 | Warning rules name has already exist
| 4001045 | Data table name has already exist
| 4001046 | Firmware oversized
| 4001047 | APN licence file oversized
| 4001048 | APP did not enable APN
| 4001049 | Product is not available to register
| 4001050 | Followed email template type had exist
| 4001051 | Lack of email template main text parameter



**Error Code Explanation: 403**

| Code | Error Code Explanation|
| --- | ---
| 4031001 | Access denied
| 4031002 | Access denied, need Access-Token
| 4031003 | Invalid Access-Token
| 4031004 | Require Enterprise allocate permission
| 4031005 | Require Enterprise admin permission
| 4031006 | Require data operate permission
| 4031007 | Private data access denied
| 4031008 | Sharing relation has been canceled
| 4031009 | Sharing relation has already been accepted
| 4031010 | Unable to operate, unable to find subscribed devices


**Error Code Explanation: 404**


| Code | Error Code Explanation|
| --- | ---
| 4041001 | URL not found
| 4041002 | Enterprise member account does not exist
| 4041003 | Enterprise member does not exist
| 4041004 | Activated enterprise member email address does not exist
| 4041005 | Product does not exist
| 4041006 | Product firmware does not exist
| 4041007 | Data point does not exist
| 4041008 | Device does not exist
| 4041009 | Device extending property does not exist
| 4041010 | Enterprise does not exist
| 4041011 | User does not exist
| 4041012 | User extension property does not exist
| 4041013 | Upgrading task does not exist
| 4041014 | Third party empower does not exist
| 4041015 | Warning rule does not exist
| 4041016 | Data table does not exist
| 4041017 | Data does not exist
| 4041018 | Sharing resource does not exist
| 4041019 | Enterprise email address does not exist
| 4041020 | App does not exist
| 4041021 | Product forward rule does not exist
| 4041022 | Email template does not exist


**Error Code Explanation: 503**

| Code | Error Code Explanation|
| --- | ---
| 5031001 |Server exception|
