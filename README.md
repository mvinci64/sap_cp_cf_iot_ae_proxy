# SAP IoT AE Node.js proxy app for the sapcp cf env. to get access from SAP Analytics Cloud

Create the user provided service via cf CLI e.g. for the OAuth credentials:

> cf create-user-provided-service my-usp-sap-proxy -p "{\"clientId\":\"<your client id>\",\"clientSecret\":\"<your client secret>\", \"tenant\":\"<your CF tenant name>\",  \"landscape\":\"eu10\", \"host\":\"hana.ondemand.com\"}"



SAP CP Cockpit at Cloud Foundry with the previous binded "user defined variable"

![Alt text](pics/udv_1.PNG?raw=true "SAP CP Cloud Fiundry service binding ")

Reference the service in the deployment descriptor:

```
---
applications:
- name: sap-proxy
  buildpack: nodejs_buildpack
  command: node app.js
  memory: 128M
  disk_quota: 128M
  host: sap-proxy
  services:
    - my-usp-sap-proxy
```

access the user defined variable(s) in your application is this way

```
var cfenv = require('cfenv')

var appEnv = cfenv.getAppEnv()

appEnv.getService('my-usp-sap-proxy').credentials.clientId

```
