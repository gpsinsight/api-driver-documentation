FORMAT: 1A
HOST: https://api.gpsinsight.com/driver/

# GPS Insight V3

Driver-centric API for GPS Insight.

## Group Session

## Authenticate [/session/authenticate/]

### Authenticate [POST /session/authenticate/]

+ Request (application/json)
    The authcode will be sent to the driver's phone number, if the phone is registered to the right refid
    
    + Body
    
            {
                "ref_id": 76,
                "phone_number": 2085678934
            }

+ Response 200

+ Request (application/json)
    If the serial number has a driver assigned, response will contain a session token

    + Body
    
            {
                "ref_id": 76,
                "serial_number": 12345678
            }

+ Response 200

        {
            "session": <session_token>
        }


### Phone Number [GET /session/authenticate/{?ref_id}{?phone_number}]

+ Parameters
    
    + ref_id: 76 (Number)
    + phone_number: 2085678934 (Number)

+ Request (application/x-www-form-urlencoded)
    The authcode will be sent to the driver's phone number, if the phone is registered to the right refid
            
    + Body
            
+ Response 200


### Serial Number [GET /session/authenticate/{?ref_id}{?serial_number}]

+ Parameters 

    + ref_id: 76 (Number)
    + serial_number: 12345678 (Number)

+ Request (application/x-www-form-urlencoded)
    If the serial number has a driver assigned, response will contain a session token
    
    + Body

+ Response 200

        {
            "session": <session_token>
        }


## Authorize [/session/authorize/]

### Session from Auth Code [POST /session/authorize/]

+ Request (application/json)
    The authcode comes from the Authenticate method, but is not delivered as a response in the API.  It is only delivered to the driver's phone.
    
    + Body
    
            {
                "ref_id": <driver_id>,
                "auth_code": <auth_code>
            }
    
+ Response 200

        {
            "session": <session_token>
        }


### Authorize Session from Auth Code [GET /session/authorize/{?ref_id}/phone{?auth_code}]

+ Parameters
    
    + ref_id: 76 (Number)
    + auth_code: abcd (String)
    
+ Request (application/x-www-form-urlencoded)
    The authcode comes from the Authenticate method, but is not delivered as a response in the API.  It is only delivered to the driver's phone.
    
    + Body
    
+ Response 200

        {
            "session": <session_token>
        }

## Group Messaging

## Messages [/message/]

### Get All Messages [GET /message/{?since}{?until}]

+ Parameters
    
    + since: `2015-08-05T08:40:51` (String,optional) - ISO8601 date
    + until: `2015-08-05T08:40:51` (String,optional) - ISO8601 date

+ Request

    + Headers
    
            Session: <Valid session token>

+ Response 200 (application/json)

        [
            {
                "id": "870h8dfhs9d8fg",
                "timestamp": "2015-08-05T08:40:51",
                "from": "John Somebody",
                "message": "Some message that is really important."
            }
        ]

### Get a Message [GET /message/{id}]

+ Parameters
    
    + id: `870h8dfhs9d8fg` (String, optional)

+ Request

    + Headers
    
            Session: <Valid session token>

+ Response 200 (application/json)

        [
            {
                "id": "870h8dfhs9d8fg",
                "timestamp": "2015-08-05T08:40:51",
                "from": "John Somebody",
                "message": "Some message that is really important."
            }
        ]

### Send a Message [POST /message]

+ Request (application/json)

    + Headers
    
            Session: <Valid session token>
            Get-Record: false
    
    + Body
    
            {
                "to": "user",
                "from": "Sum Yung Gai",
                "from_type": "driver",
                "device_type": "mobile",
                "message": "A message"
            }

+ Response 200 (application/json)

        {
            "id": 123456
        }

+ Request (application/json)

    + Headers
    
            Session: <Valid session token>
            Get-Record: true
    
    + Body
    
            {
                "to": "user",
                "from": "Sum Yung Gai",
                "from_type": "driver",
                "device_type": "mobile",
                "message": "A message"
            }

+ Response 200 (application/json)

        {
            "id": 123456
            "to": "user",
            "from": "Sum Yung Gai",
            "from_type": "driver",
            "device_type": "mobile",
            "message": "A message"
        }


## Message Receipt [/message/receipt/]

### Send Message Receipt [POST /message/receipt/{id}]

+ Parameter

    + id: 1234 (String) - ID of message read

+ Request

    + Headers
    
            Session: <Valid session token>

+ Response 200


### Send Multiple Receipts [POST /message/receipt/]

+ Request

    + Headers
    
            Session: <Valid session token>
            
    + Body
    
            [
                {
                    "id": 123
                },{
                    "id": 124
                },{
                    "id": 125
                }
            ]

+ Response 200

## Group Dispatching

## Dispatch [/dispatch{?since}]

### Get All Dispatches [GET]

+ Parameters

    + since: `2015-08-05T08:40:51` (String, optional) - ISO8601 date

+ Request

    + Headers
    
            Session: <Valid session token>

+ Response 200 (application/json)

        [
            {
                "id": "<Dispatch ID>",
                "latitude": 121.191,
                "longitude": -100.231,
                "address": "3210 N Butte St. Bremerton, WA 96753",
                "note": "A really important note. Their dog will murder you.",
                "timestamp": "2015-08-05T08:40:51"
            }
        ]

## Dispatch Receipt [/dispatch/receipt/{id}]

### Send Dispatch Receipt [POST]

+ Parameters

    + id: 1234 (String) - ID of message read

+ Request

    + Headers
    
            Session: <Valid session token>
            
    + Body
            
            {
                "id": "DispatchID"
            }
            
+ Response 200
