FORMAT: 1A
HOST: https://api.gpsinsight.com/driver/

# GPS Insight V3

Driver-centric API for GPS Insight.

## Group Session

## Authenticate [/session/authenticate/]

### Authenticate Driver [POST /session/authenticate/]

+ Request (application/json)
    The auth_code will be sent to the driver's phone number, if the phone is registered to the right ref_id
    
    + Body
    
            {
                "ref_id": 76,
                "phone_number": 2085678934
            }

+ Response 200

+ Request (application/json)
    If the hardware_key has a driver assigned, response can contain a session token, doing the session generation in one step.

    + Headers
    
            Get-Record: true
    + Body
    
            {
                "ref_id": 76,
                "hardware_key": 123456789101112
            }

+ Response 200

        {
            "session": <session_token>
        }


### Session from Auth Code [GET /session/authorize/{auth_code}]

+ Parameters
    
    + auth_code: abcd (String)
    
+ Request (application/json)
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
