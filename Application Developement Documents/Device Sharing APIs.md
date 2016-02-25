# Device Sharing APIs

With device sharing api, it allow user to manage sharing control, including accept, deny, cancel,etc.



## At a Glance

1. [Device Sharing](#device_share)
2. [Cancel Sharing](#device_share_cancel)
3. [Accept Sharing](#accept_share)
4. [Deny Sharing](#deny_share)
5. [Retrieve Sharing Device List](#share_list)
6. [Delete Sharing History](#delete_share)


## Interface Details

### **<a name="device_share">1.Device Sharing</a>**


User can share devices to other users, allow other users to control certain devices. Sharer will be the manager, other user will be general users. Only device manager can share.



#### ***Device Sharing Workflow***
	Sharer and User:
	1.Sharer send invitation to a certain User
    2.System will create a record in Sharing Request List in both Sharer and User，the request will be invalid after a certain period.
    3.User select [Accept] or [Deny] from the Sharing Request List.
    4.Sharer are able to cancel invitation from the Sharing Request List before User accept or deny.
    5.User accept invitation and share the same device as the Sharer.



##### ***3 Ways To Share：***

* Share via User ID
* QR Code Sharing
* Share Via Email


##### ***About Device Manager***

Only register and subscribe via Xlink Platform SDK can become a device manager.User register a new device to become the device manager.

![](http://i.imgur.com/pYdAaZN.png)

**Request**


URL

    POST /v2/share/device

Header

    Content-Type : "application/json"
    Access-Token : "Access Token"

Content

    {
        "device_id" : Device ID,
        "user" : "User Account Name",
        "expire" : 7200,
        "mode" : "Sharing Mode"
    }


Variables | Required | Description
---- | ---- | ----
device_id | Yes | Sharing Device ID
user | NO | Whom to share;Can be a phone number or an email address (If share via QR code or email, only invite code is required.)
expire | Yes | Request valid period, in seconds
mode | Yes | Sharing Mode，enumeration value，See [Appendix](#share_mode)

**Response**

Header

    HTTP/1.1 200 OK

Content

    {
    "invite_code" : "Invite code"
    }


### **<a name="device_share_cancel">2.Cancel Sharing</a>**



	Device manager can cancel an invitation via this interface.


**Request**

URL

    POST /v2/share/device/cancel

Header

    Content-Type : "application/json"
    Access-Token : "Access Token"

Content

    {
        "invite_code" : "Invite Code",
    }

**Response**

Header

    HTTP/1.1 200 OK

Content

    Null



### **<a name="accept_share">3.Accepting Sharing</a>**


    User accept sharing.

**Requeset**

URL

    POST /v2/share/device/accept

Header

    Content-Type : "application/json"
    Access-Token : "Access Token"

Content

    {
    "invite_code" : "Invite Code"
    }

**Response**

Header

    HTTP/1.1 200 OK

Content

    NULL



### **<a name="deny_share">4.Deny Sharing</a>**


    User deny sharing.

**Request**

URL

    POST /v2/share/device/deny

Header

    Content-Type : "application/json"
    Access-Token : "Access Token"

Content

    {
        "invite_code" : "Invite Code",
        "reason":"Reason user deny sharing"
    }


Variables | Required | Description
---- | ---- | ----
invite_code | Yes | Invite Code
reason | No | Reason user deny sharing

**Response**

Header

    HTTP/1.1 200 OK

Content

    NULL



### **<a name="share_list">5.Retrieve Sharing Device List</a>**



    Device Manager and user can check devices sharing list: Device shared to others and shared by others.

**Request**

URL

    GET /v2/share/device/list

Header

    Content-Type : "application/json"
    Access-Token : "Access Token"

Content
    无

**Response**

Header

    HTTP/1.1 200 OK

Content

    [
        {
            "invite_code" : Invite Code,
            "from_id" : Sharer ID,
            "from_user" : Sharer Account,
            "to_id" : User ID(to whom),
            "to_user" : User Account(to whom),
            "device_id" : Device ID,
            "state" : Sharing Status
            "create_date" : Share create time，
            "expire_date" : Share expire time，
        },
    ]

Variables | Required | Description
---- | ---- | ----
invite_code | Yes | Invite Code
from_id | Yes | Sharer Id(Device Manager's ID)
from_user | Yes | Sharer Account(Device Manger's ID)
to_id | Yes | User ID(Share to Whom)
to_user | Yes | User Account Name(Share to Whom)
device_id | Yes | Device ID
state | Yes | Sharing Status;See[Appendix](#share_status)
create_date | Yes | Share create time, for example：2015-10-09T08 : 15 : 40.843Z
expire_date | Yes | Share expire time, for example：2015-10-09T08 : 15 : 40.843Z


### **<a name="delete_share">6.Delete Sharing History</a>**


    Device Manager or User delete sharing history

**Request**

URL

    DELETE /v2/share/device/delete/{invite_code}
Header

    Content-Type : "application/json"
    Access-Token : "Access Token"

Content

    NULL

**Response**

Header

    HTTP/1.1 200 OK

Content

    NULL
