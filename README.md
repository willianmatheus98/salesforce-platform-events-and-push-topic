# Deploy

Go to the manifest/package.xml and click-right button on the file and select "SFDX: Deploy Source in Manifest to Org"


## Create the push topic

```java
PushTopic pushTopic = new PushTopic();
pushTopic.Name = 'CasesClosed';
pushTopic.Query = 'SELECT Id, CaseNumber, Status, AccountId, Comments FROM Case WHERE IsClosed = true';
pushTopic.ApiVersion = 53.0;
pushTopic.NotifyForOperationCreate = true;
pushTopic.NotifyForOperationUpdate = true;
pushTopic.NotifyForOperationUndelete = true;
pushTopic.NotifyForOperationDelete = true;
pushTopic.NotifyForFields = 'Referenced';
insert pushTopic;
```

## Subscribe by CometD

Clone this repository: https://github.com/forcedotcom/EMP-Connector

Go on the folder downloaded and execute the following commands

### Platform event
```bash
java -jar target/emp-connector-0.0.1-SNAPSHOT-phat.jar <username> <password> "/event/CaseClosed__e"
```

### Push topic
```bash
java -jar target/emp-connector-0.0.1-SNAPSHOT-phat.jar <username> <password> "/topic/CasesClosed"
```

Reference: https://developer.salesforce.com/docs/atlas.en-us.platform_events.meta/platform_events/code_sample_java_download.htm

### Now close your Cases and the event messages will be shown in the terminal 😎 like below

```json
{
    "event": {
        "createdDate": "2022-01-13T14:47:17.541Z",
        "replayId": 2,
        "type": "updated"
    },
    "sobject": {
        "Status": "Closed",
        "AccountId": "0015Y00002iqcmQQAQ",
        "Comments": "Test",
        "CaseNumber": "00001029",
        "Id": "5005Y000023Xg86QAC"
    }
}

```