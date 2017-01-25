# CAM alerting using TR- Logging

### This document gives a brief about how to raise and manage alarms pusblishing to the TR-Logging ingest topics

CAM dashboard alarms from TR Logging Schema compatible events.
Alarming events: "sp-isAlarm": true,
Turns alarming event json into an alarming episode:

## How does it work?
- Application sends TR Logging Schema based Alarming Event.
- Alarming events: "sp-isAlarm": true -> translates event to alarms
- Alarm should be available in TRAMS ELK .
- Compass Event Collector [details](https://thehub.thomsonreuters.com/docs/DOC-851597)
- To implement TR- Logging refer [TR-Logging Project ](https://git.sami.int.thomsonreuters.com/TR-Enterprise-Logging/TR-Logging-Project)
- For CAM details refer [TR-Alarm Management](https://git.sami.int.thomsonreuters.com/TR-Enterprise-Logging/TR-Logging-Project/blob/master/TR-Alarm-Management-Service.md)

## TR Logging Extensions ([Example Project](https://git.sami.int.thomsonreuters.com/TR-Enterprise-Logging/java-TR-Logging-Project-example)) 
1. Register the application in the Software Module Registry
    - Pre-requisite: sp-applicationUniqueId : Asset Insight Unique ID.
    - More info : [Software Module Registration](https://git.sami.int.thomsonreuters.com/TR-Enterprise-Logging/TR-SoftwareModuleRegistry-entries)
2. Include necessary internal TR Enterprise Logging and third-party libraries in the application build path
3. Application logging configuration: set up TR Enterprise Logging Kafka  appender (e.g. EnterpriseKafkaLog4jAppender) and JSON event layout (e.g. Log4jJsonEventLayout)
4. Add the necessary JVM parameters to the application’s startup or put required the properties in application code. 
5. Add logging logic to application code 
6. Log events will show up in Kibana dashboard, Alarm events will show up in CAM.[CAM Dashboard]


#### For Dependencies (Gradle Format),Application Logging Configuration and SM Registry properties specified as VM argument refer this [link](https://thehub.thomsonreuters.com/servlet/JiveServlet/downloadBody/1850624-102-23-4883388/Logging_Schema_Based_Alarms_for_ECP-v10%20%282%29.pptx)

#### Example of the resulting alarm refer this [link](https://thehub.thomsonreuters.com/servlet/JiveServlet/downloadBody/1850624-102-23-4883388/Logging_Schema_Based_Alarms_for_ECP-v10%20%282%29.pptx)



### Alarm correlation: For grouping alarms to episodes. 
- Group related alarms together: alarming episodes
- Alarm Correlation Signature:

#### Correlation_signature 
Correlation id which identifies an episode as a unique entity.
For example please refer this [document](https://thehub.thomsonreuters.com/servlet/JiveServlet/downloadBody/1850624-102-23-4883388/Logging_Schema_Based_Alarms_for_ECP-v10%20%282%29.pptx).

#### Troubleshooting with Postman and Using Postman to verify CAM alarming event 
HOW TO INSTALL IT:
1. Point your browser https://chrome.google.com/webstore/search/postman?_category=apps
2. Click  "ADD TO CHROME"
3. Go to Settings/Extensions
4. Locate Postman and click Details
5. Click Create shortcuts  

You will have a flying spaceman icon on your desktop and taskbar


HOW TO USE IT TO VALIDATE TRLog JSON Object
1. Select POST and type in the (Development) URL for API http://compass-alarm-dev-01.emea1.cis.trcloud:3001/alarm_api/v2/events
2. Add a Header in the Headers section
```
Key : Content-Type 
Value: application/json
```
3. Select raw in the Body section
4. Paste json object into the box 
```
{  
 "sp-isAlarm": true,
  "sp-applicationUniqueID": "203998",
  "sp-eventSourceUUID": "30044842-21c9-11e6-b67b-9e71128cae77",
  "sp-threadName": "http-bio-8080-exec-3",
  "sp-eventType": "trmr",
  "sp-timestamp": "2017-01-24T14:40:30.477Z",                                  
  "sp-eventSourceVersion": "3.2",
  "sp-eventGroupID": "eventID-1-3-2-4",
  "sp-emailContactList": "ct-sdp-dev@thomson.com",
  "sp-userID": "tlr-243",
  "sp-eventSchemaVersion": 3,
  "sp-softwareModuleName": "SabyaHelloWorldTRLoggingTest",
  "sp-message": "Sabya-HelloWorldTest",
  "sp-functionalDomain": "ct-sdp",
  "sp-eventContext": {
    "sp-environmentClass": "development",
    "sp-eventSourceHostname": "c123abc.int.thomsonreuters.com",
    "sp-buildingName": "GB-DTC",
    "sp-hostingModuleID": "H12345",
    "sp-buildingID": "B00312",
    "sp-hostingModuleName": "Eagan-DTC",
    "testAttribute": "http://tr.com/wlnnext",
    "sp-environmentLabel": "demo"
  },
  "sp-eventSeverity": "warning"                                
}
```
5.	Click Send to generate warning alarm
6.	Change    "sp-eventSeverity": "ok"
7.	Click Send to clear the alarm
8.	Check if the alarm is created  on this link http://compass-alarm-dev-01.emea1.cis.trcloud:3000


### Clearing alarms

- You can auto-clear alarms via application logging
- You can manually clear alarms from Dashboard
- Alarms are archived (disappearing from live dashboard)
  - Critical 4 days after last update
  - Warning 2 days after last update
  - OK 1day after last update
- Archived alarms are kept 6 months
- You can access archived alarms from Reporting tool


#### Auto CLear
To auto clear a previous warning/critical alarm, you must send an alarm with OK severity and same correlation id ( as critical/warning) driven from a number of fields as described in Correlation id.

#### To clear alarm manually:
It does not have to be a manual process. However, the dashboard allows you to take ownership of an alarm and change severity to OK.

[Clearing alarm details](https://thehub.thomsonreuters.com/docs/DOC-851597#jive_content_id_Clearing_alarms)</br>



### How to check Alarms after sending event from TR Logging Extensions
1. Go to [CAM dashboard](https://compass.thomsonreuters.com/monitor/alarms/)
2. Click on the Show Filters img at top left corner
3. Click on Default tab and clear all the filters for default
4. Click on Application tab and type in your application name and enter.
      - Hover on + symbol of SSDF and click on the event type(critical in this case)
5. Will get the application and count of events based on level.
      - Click on the aplication 
6. GIves further details about the perticular event with details .

Refer this [link](https://thehub.thomsonreuters.com/docs/DOC-851597#jive_content_id_Testing) for more details.



### References

[The Thomson Reuters Enterprise Logging Framework](https://git.sami.int.thomsonreuters.com/TR-Enterprise-Logging/TR-Logging-Project)</br>
[CAM dashboard](https://compass.thomsonreuters.com/monitor/alarms/)</br>
[CAM alarm from TRLogModel](https://thehub.thomsonreuters.com/docs/DOC-851597)</br>
[Developers Guide](https://git.sami.int.thomsonreuters.com/TR-Enterprise-Logging/TR-Logging-Project/blob/master/TR_Logging_Tutorial.md) </br>
[TR Logging-Compass Monitor Integration](https://thehub.thomsonreuters.com/docs/DOC-1850624) </br>
[FAQs](https://thehub.thomsonreuters.com/docs/DOC-1079951) </br>

