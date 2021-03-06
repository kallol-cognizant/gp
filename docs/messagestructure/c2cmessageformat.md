#C2C Message Format   

**Message Name** - C2C Message Format.<br>
**Description** - C2C Message, a top level message for all communication with and from device along with common headers. The transport layer between the vehicle and cloud is MQTT, which supports only the publish/subscribe messaging pattern. MQTT does not have any inherent mechanism for correlating sequences of messages, hence C2C Message includes `message_id` and `correlation_id` attributes for this purpose. <br>
**Structure** - Json<br>
**Header Size** - approx 460 bytes<br>

## Message
=== "V1"

   ```json
   {
      "message_id": "<source><UUID>",
      "correlation_id": "<source><UUID>",
      "version": "v1",
      "system_id": "sys<UUID>",
      "sub_system_id": "sub<UUID>",
      "vin": "*** vin ***",
      "device_id": "*** device Identifier ***",
      "ecu_type": "**ecu type**",
      "source_id": "c2c-cloud",
      "target_id": "c2c-edge",
      "message_type": " ** message type ** ",
      "time": 1620891851,
      "ttl": -1,
      "status": "SUBMIT",
      "propertyBag": {
         "bodyEncodingType": 1
      },
      "body": // Datatype string, below object will be base64 Encoded String
      {}
    }
   ```
=== "V2"

   ```json
   {
      "message_id": "<source><UUID>",
      "correlation_id": "<source><UUID>",
      "version": "v2",
      "system_id": "sys<UUID>",
      "sub_system_id": "sub<UUID>",
      "device_id": "*** device Identifier ***",
      "source_id": "c2c-cloud",
      "target_id": "c2c-edge",
      "message_type":** message type ** ,
      "time": 1620891851,
      "ttl": -1,
      "propertyBag": {
         "bodyEncodingType": 1
      },
      "body": // Datatype string, below object will be base64 Encoded String
      {}
    }
   ```
### Headers - V1

|Attribute Name|Datatype|Mandatory| Length (bytes) |Description|
| :------------- | :------------ |:------------ |:------------: |:------------ |
|message_id|String|Yes| 23 |Unique Id generated by originator.Message id Format is {source}Unique id. Source can be either DEV for Device and CLD for Cloud. While a particular implemenation may make messageId a monotonically increasing unique sequence, it is not a requirement to do so and applications should not count on such behavior.|
|corelation_id|String|No| 23 |Device replying / responding to the message received from Cloud. Value is the message_id provided by the cloud message. While a particular implemenation may make messageId a monotonically increasing unique sequence, it is not a requirement to do so and applications should not count on such behavior.|
|version|String|Yes| 2 |Message schema version|
|system_id|String|Yes| 23 |Unique id generated at time of Registration for the system. Format is  sysUnique id.|
|sub_system_id|String|No| 10 |Unique id generated for the sub system. Format is  subUnique id.|
|vin|String|No| 17 |Vehicle Identification number.vin is mandatory for appropriate homing|
|device_id|String|Yes| 19 |Manufacturer unique serial no/device_id.|
|ecu_type|String|Yes| 10 |Type of the ECU. Possible values see below ECU Type table. |
|source_id|String|Yes| 10 |Source application Id.Use defaut value as c2c-edge / c2c-cloud if not known. |
|target_id|String|Yes| 10 |Target application id.Use defaut value as c2c-edge / c2c-cloud if not known. |
|message_type|String|Yes| 50 |Value of Message Type specific to the application. Refer Message Type table below for possible values. |
|time|Numeric|Yes| millisecond |Message Creation Unix Timestamp in milliseconds. |
|ttl|Numeric|No| |Time to Live in SECONDS, -1 (default) is no expiry. Expiry time should be a positive number, greater than Zero. |
|status|String|Yes| |Staus of message operation. Values SUCCESS, FAILED etc.|
|property_bag:body_encoding_type|Numeric|Yes| |Value represents the type of encoding used on body. 1 = Base64.|
|body|String|Yes| |Encoded message.|
    
### Headers - V2

|Attribute Name|Datatype|Mandatory| Length (bytes) |Description|
| :------------- | :------------ |:------------ |:------------: |:------------ |
|message_id|String|Yes| 23 |Unique Id generated by originator.Message id Format is {source}Unique id. Source can be either DEV for Device and CLD for Cloud. While a particular implemenation may make messageId a monotonically increasing unique sequence, it is not a requirement to do so and applications should not count on such behavior.|
|corelation_id|String|No| 23 |Device replying / responding to the message received from Cloud. Value is the message_id provided by the cloud message. While a particular implemenation may make messageId a monotonically increasing unique sequence, it is not a requirement to do so and applications should not count on such behavior.|
|version|String|Yes| 2 |Message schema version|
|system_id|String|Yes| 23 |Unique id generated at time of Registration for the system. Format is  sysUnique id.|
|sub_system_id|String|No| 10 |Unique id generated for the sub system. Format is  subUnique id.|
|device_id|String|Yes| 19 |Manufacturer unique serial no/device_id.|
|source_id|String|Yes| 10 |Source application Id.Use defaut value as c2c-edge / c2c-cloud if not known. |
|target_id|String|Yes| 10 |Target application id.Use defaut value as c2c-edge / c2c-cloud if not known. |
|message_type|Integer|Yes| 50 |Value of Message Type code specific to the application. |
|time|Numeric|Yes| millisecond |Message Creation Unix Timestamp in milliseconds. |
|ttl|Numeric|No| |Time to Live in SECONDS, -1 (default) is no expiry. Expiry time should be a positive number, greater than Zero. |
|property_bag:body_encoding_type|Numeric|Yes| |Value represents the type of encoding used on body. 1 = Base64.|
|body|String|Yes| |Encoded message.|
    	
## Additional Info

### ECU Type
|Name|Description|
|:-------------|:-------------|
|TCU| |
|IVI| |
|ADAS| |

### Message Types
|Name| Identifier |Description|
|:-------------|:-------------|:-------------|
|PROVISION_QUERY| | |
|PROVISION_QUERY_RESPONSE| | |
|DEVICE_PROVISION| | |
|DEVICE_PROVISION_RESPONSE| | |
|CHIPSET_ONBOARDING_NONCE| | |
|CHIPSET_ONBOARDING_NONCE_RESPONSE| | |
|CHIPSET_NONCE| | |
|CHIPSET_NONCE_RESPONSE| | |
|CHIPSET_INSTALL_LICENSE| | |
|CHIPSET_INSTALL_LICENSE_RESPONSE| | |


## Message Type Codes
  INVALID =0, <br/>
  PROVISION_QUERY=1,<br/>
  PROVISION_QUERY_ACK=2,<br/>
  PROVISION_QUERY_RESPONSE=3,<br/>
  DEVICE_PROVISION=4,<br/>
  DEVICE_PROVISION_ACK=5,<br/>
  DEVICE_PROVISION_RESPONSE=6,<br/><br/>

  CHIPSET_ONBOARDING_NONCE=51,<br/>
  CHIPSET_ONBOARDING_NONCE_ACK=52,<br/>
  CHIPSET_ONBOARDING_NONCE_RESPONSE=53,<br/>
  CHIPSET_ONBOARDING_RESPONSE=54,<br/>
  CHIPSET_ONBOARDING_RESPONSE_ACK=55,<br/><br/>

  CHIPSET_NONCE=101,<br/>
  CHIPSET_NONCE_ACK=102,<br/>
  CHIPSET_NONCE_RESPONSE=103,<br/><br/>

  CHIPSET_INSTALL_LICENSE=151,<br/>
  CHIPSET_INSTALL_LICENSE_ACK=152,<br/>
  CHIPSET_INSTALL_LICENSE_RESPONSE=153,<br/>
  CHIPSET_UPDATE_LICENSE=154,<br/>
  CHIPSET_UPDATE_LICENSE_ACK=155,<br/>
  CHIPSET_UPDATE_LICENSE_RESPONSE=156,<br/>
  CHIPSET_DELETE_LICENSE=157,<br/>
  CHIPSET_DELETE_LICENSE_ACK=158,<br/>
  CHIPSET_DELETE_LICENSE_RESPONSE=159,<br/><br/>

  OTA_TRIGGER=201,<br/>
  OTA_TRIGGER_ACK=202,<br/>
  OTA_EVENT=203,<br/><br/>

  COMMAND=251,<br/>
  COMMAND_ACK=252,<br/>
  COMMAND_RESPONSE=253,<br/><br/>

  INFO = 301,<br/>
  DRIVER_INFO = 302,<br/>
  NAVIGATION = 303,<br/>
  TELEMETRY = 304,<br/>
  BODY_STATUS = 305,<br/><br/>

### Encoding Type
|Name|Description|
|:-------------|:-------------|
|0 | No encoding. |
|1 | Encoding type Base64|

### Registration States
|Name|Description|
|:-------------|:-------------|
|REG| Entities are enrolled  and are ready. Applies to Vehicle, Subsytem, Device and Chipset |
|UNREG| Entities are not enrolled. Applies to Vehicle, Subsytem, Device and Chipset |
|REG_FAIL| Entities failed the enrollment process. Applies to Vehicle, Subsytem, Device and Chipset |

### Provisioning states

|Name|Description|
|:-------------|:-------------|
|QRY_FAIL| Rehoming operation failed. Applies at vehicle level (i.e. primary) |
|GLOBAL| Rehoming operation cannot identify homing region. Applies at vehicle level (i.e. primary) Registered in GLOBAL Partition and Pending Provisioning in GLOBAL instance. |
|REGIONAL| Rehomed to region and pending provisioning for all devices. Applies at vehicle level (i.e. primary). Registered in REGIONAL Partition and Pending Provisioning |
|PROVISION | Entity identified successfully. Applies to Vehicle, Subsystem and Device. |
|PROV_FAIL| Entity identification failed. Applies to Vehicle, Subsystem and Device. |
|PROV_PEN | Intermediate states for entities. Applies to Vehicle only.  |
|ACTIVE| Entity is available for further operations / Service. Applies to Device & Chipset |


