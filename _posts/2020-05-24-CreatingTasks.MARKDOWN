---
layout: page
title:  "Creating Tasks Using SAP Health for Patient Engagement"
date:   2020-05-23 13:14:19 +0200
categories: jekyll update
permalink: "sampledocumentation"
---

## Table of Contents

- [About SAP Health for Patient Engagement](#about-sap-health-for-patient-engagement)
- [Authentication](#authentication)
- [Overview](#overview)
- [Structure and Build your task](#structure-and-build-your-tasks)
   - [Structure your Requirements](#structure-your-requirements)
   - [Create an XML for your Requirement](#create-an-xml-for-your-requirement)
   - [Understand the XML](#understand-the-xml)
- [Specify Units of Measure](#specify-units-of-measure)
- [Stage and Activate Tasks](#stage-and-activate-tasks)





## About SAP Health for Patient Engagement

SAP Health for Patient Engagement is a cloud solution that enables physicians to monitor the health of patients with specific health conditions. For example, if a patient has health condition insomnia, the physician can monitor the patient's sleep and heart rate. The ability to constantly monitor a patient's health helps the physician in taking the right course of action at the right time. 

The solution includes a user interface that enables physicians to monitor the health of their patients, and a set of APIs that you can use in your patient's mobile app to send health information such as average heart rate to the physician. 

Click [here](https://sapvideoa35699dc5.hana.ondemand.com/?entry_id=1_6c5a4hlc) a video overview.


## Authentication
The APIs for SAP Health for Patient Engagement use the OAuth 2.0 standard authentication. See [https://oauth.net/2/](https://oauth.net/2/)  for any additional information.


### Procedure

1. Go to [https://www.sap.com/hep](https://www.sap.com/hep) and log in with your user name and password. In the *Authentication* tab page, note down the *client_id* and *client_secret*. Do not share these credentials with any external party you do not trust.  
2. Make the following [API call](https://nikhil-mukunda.github.io/SME/#/Obtain%20bearer%20token/GetBearer) with the _client_id_ and _client_secret_.
3. Make a note of the response you receive. The response for the API call should look like this:
```
{
"access_token":" utTJFMrOKwlyB5rcBpCIP6Dtn0k4w8vtqR6TJtu-fvEIm9tXTZf6q4JSaRaxRc7eSgO4EAggELN5bqADCSGq4mDEQgM-k-VPUi7IVIkKrVAFdwyb9Yz1cXy9pU96tZSmxjNiNzMiLCJvcmciOiJTTFI6MTMyMjAzNiIsImF6cCI6IjMwODE4NWVhLTkyZTAtNGE0NS1iNzg2LWU5MzI1OTIyM2I3MyIsImNsaWVudG5hbWUiOiJDaGFubmFWFmYyRdvKZyB4PibGUiLCJpc3MiOiJsb2dpbi5ib2wuY29tIiwic2NvcGVzIjoiQ3VzdG9tU2NvcGU0IEN1c3RvbVNjb3BlMyIsImV4cCI6MTUzOTI0NDkwMSwiaWF0IjoxNTM5MjQ0NjAxLCJhaWQiOiJTTFI6MTMyMjAzNiIsImp0aSI6IjI1YzcxZDY2LTU0YzgtNDQ3ZS04NTk0LWEwMzFlZDNkNGUzOSJ9.Nkc90mTB-BLVJEnSDHx2o1bkJyJraWQiOiJyc2ExIiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiIzMDgxODVlYS05MmUwLTRhNDUtYjc4Ni1lOTMyNTkyM7Dan9VaFQF1o8EvlGCV42n61KAjeEg8PrjVwqFvJ8y9QUzcpTFXQ5f4VFgIfZfYaqZyM2iJWFlpSpVl-jQAiGjOp0xSForKtGe2-FdyXmmQNpw_IltcPmvJIGABU3Xngx5O-13sG_zRoy7g1CBspU9dx5DLDuOa17PBmj52kQVEV8Q",
"token_type": "Bearer",
"expires_in": 299,
"scope": "{scopes}"
}
```

For every API call you make to the SAP Health for Patient Engagement system, you must request a new access token as shown in step 2.


## Overview

### Use
You create tasks that a patient performs for a particular condition. These tasks can then be monitored by the patient's physician. For example, for the condition insomnia, you create tasks for the patient to enter hours of sleep, average heart rate, and type of sleep every day. The physician can then monitor these tasks and decide on a course of treatment for the patient. 

Note that you can define logic in your mobile app so that all of this information is captured from the patient's health device such as activity trackers. This helps the patient to avoid manually entering data into your app. 

### About this guide

This guide is intended primarily for developers. It is a supplement to our [technical documentation](https://nikhil-mukunda.github.io/APIDocs/), which should be consulted for more detailed information on all endpoints. However, we recommend that you read this documentation before you look at the technical documentation. 

### Prerequisites


* You have a basic understanding of software development and RESTful APIs. 
* You have a basic understanding of SAP Health for Patient Engagement. For more information, see [About SAP Health for Patient Engagement](#about-sap-health-for-patient-engagement).
* You have the bearer token for authentication. For more information, see [Authentication](#authentication). 

### Procedure

1. [Structure and build your task.](#structure-and-build-your-tasks)
2. [Upload your task to a staging area, and activate the task.](#stage-and-activate-tasks) 



## Structure and Build your Tasks 

You can structure and build your task with XML. This topic provides an example that specifies tasks for condition insomnia. You can create your task based on this example. 

For the condition insomnia, a patient performs tasks such as entering the hours of sleep every night, the average heart rate, the type of sleep (restless, good, very deep), and the environment during sleep (humid, cold, hot, quiet, and noisy).

Use the following sections to create your task:

###  Structure your requirements
Structure your requirements based on the following elements:
1. The condition of the patient and the attributes: 
*  Unique Identifier (ID) for the condition 
*  Description of the condition 
*  Translations of the title and description in required languages
    
2. Tasks with numerical values:
  *   Title of the task
  *   ID of the task
  *  Title of the task in various languages
  * Units of measurement ID (UoM).  
   You must define units of measure before you start creating tasks. For more information, see [Specifying units of measure](#specify-units-of-measure)   
  *   Default unit of measurement (default UoM) or standard unit
  * Upper normal, lower normal, hyper and hypo in various languages. For more information, see the topic *Tasks*  under  *Apps for Administration* -> *Manage Programs*  in the application help in  [https://help.sap.com/viewer/p/PATIENT_ENGAGEMENT](https://help.sap.com/viewer/p/PATIENT_ENGAGEMENT "https://help.sap.com/viewer/p/PATIENT_ENGAGEMENT")   
  *   Total number of digits you require for the value (Precision)   
  *   Number of fractional digits you require (Scale)

3. Tasks with fixed values:
  * Title & ID of the task 
  *  Fixed values & IDs of the tasks
  *  Order of the values in the user interface    
  *  Multiple select or single select
  *  Translation of the texts in required languages

#### Example

For patients with the condition insomnia, the following tasks are created: 
* Amount of Sleep
 * Average Heart Rate
 * Type of Sleep  
*  Environment
It is also for patients who are Spanish and French.     

The following depicts the requirement structure of the example:

1. The condition of the patient and the attributes:
    
    * ID:  *condition_insomnia*
     *  Title:
        *   English: Insomnia  
        *   French:  Insomnie
        *   Spanish: Insomnio
            
    *   Description:       
        *   English: Enables you to check sleep behavior and associated attributes            
        *  French: Permet de vérifier le comportement du sommeil et les attributs associés
        *   Spanish: Le permite comprobar el comportamiento del sueño y atributos asociados
            
2. Tasks that have numerical values:

|Title and ID|UoM|Precision, Scale|Lower Normal, Upper Normal| Hyper, Hypo|
|--|--|--|--|--|--| 
| ID: SLEEP_SLEEPAMOUNT  <br/> Title:  <br/> EN: Sleep Amount <br/> FR: Somme du sommeil   <br/>     ES: Cantidad de sueño | *SLEEPAMOUNT_STDUNIT_HR* <br/> *SLEEPAMOUNT_STDUNIT_MIN* <br/> Default: *SLEEPAMOUNT_STDUNIT_HR* | precision: 4 <br/> scale: 1 |  Upper Normal Value: 9 <br/>Title:<br/>EN: Upper Normal<br/>FR: Valeur normale supérieure<br/>ES: Valor normal superior<br/><br/>Lower Normal Value: 6 <br/>  Title: <br/>EN: Lower Normal Value<br/>FR: Valeur normale inférieure<br/> ES: Valor normal inferio |Hyper Value: -<br/>Title: -<br/><br/> Hypo Value: 2<br/> Title:<br/>EN: Hypo <br/>FR: Hypo <br/>ES: Hipo | 
| ID: SLEEP_HEARTRATE  <br/> Title:  <br/> EN: Heart Rate <br/> FR: Rythme Cardiaque   <br/>ES: Ritmo Cardiaco | *HEARTRATE_STDUNIT_BPM* <br/> Default: *HEARTRATE_STDUNIT_BPM* | precision: 3 <br/> scale: 0 |  Upper Normal Value: 80 <br/>Title:<br/>EN: Upper Normal<br/>FR: Valeur normale supérieure<br/>ES: Valor normal superior<br/><br/>Lower Normal Value: 60 <br/>  Title: <br/> EN: Lower Normal Value<br/>FR: Valeur normale inférieure<br/> ES: Valor normal inferio |Hyper Value: 100<br/> Title: <br/> EN: Hyper<br/>FR: Hyper<br/>ES: Hiper<br/><br/>Hypo Value: 50<br/> Title:<br/>EN: Hypo <br/>FR: Hypo <br/>ES: Hipo 
3. Tasks with fixed values:

|ID <br/>  Title|Multiple <br/> Select |Fixed Values: <br/>ID<br/> Order <br/> Title|
|--|--|--|--|
|ID: SLEEP_TYPEOFSLEEP <br/>  Title: <br/> EN: Type of Sleep <br/> FR: Type de sommeil <br/>ES: Type de sommeil | No (value: 1) | ID: TYPEOFSLEEP_VALUE_RESTLESSSLEEP <br> Order: 1 <br> Title: <br/> EN: Restless Sleep <br/>  FR: Sommeil agité <br/> ES: Sueño inquieto <br/><br/> ID: TYPEOFSLEEP_VALUE_GOODSLEEP <br> Order: 2<br/>  Title: <br/> EN: Good Sleep <br/> FR: Bien sommeil <br/>ES: Buen sueño <br/><br/> ID: TYPEOFSLEEP_VALUE_DEEPSLEEP <br> Order: 3<br/>  Title: <br/> EN: Deep Sleep <br/> FR: Sommeil profound <br/>ES: Sueño profundo
|ID: SLEEP_ENVIRONMENT <br/>  Title: <br/> EN: Sleep Environment <br/> FR: Environnement <br/>ES: Ambiente | Yes (value: 2) | ID: ENVIRONMENT_VALUE_HUMID  <br> Order: 1 <br> Title: <br/> EN: Humid <br/>  FR: Humide <br/> ES: Húmedo <br/><br/> ID: ENVIRONMENT_VALUE_COLD <br> Order: 2<br/>  Title: <br/> EN: Cold <br/> FR: Du froid <br/>ES: Frío <br/><br/> ID: ENVIRONMENT_VALUE_HOT <br> Order: 3<br/>  Title: <br/> EN: Hot <br/> FR: Chaud <br/>ES: Caliente <br/><br/> ID: ENVIRONMENT_VALUE_QUIET <br> Order: 4<br/>  Title: <br/> EN: Quiet <br/> FR: Silencieux <br/>ES: Tranquilo<br/><br/> ID: ENVIRONMENT_VALUE_NOISY <br> Order: 5<br/>  Title: <br/> EN: Noisy <br/> FR: Bruyant<br/>ES: Ruidoso

### Create an XML for your requirement
You can [download](https:www.sap.com/support/hep) an XSD file and generate an XML template. You can then create your XML from the template. The following XML is for the condition insomnia:

```
<?xml version="1.0" encoding="UTF-8"?>
<Measurement id="condition_insomnia">
  <title language="EN">Insomnia</title>
  <title language="FR">Insomnie</title>
  <title language="ES">Insomnio</title>
  <description language="EN">Enables you to check sleep behavior and associated attributes.</description>
  <description language="FR">Permet de vérifier le comportement du sommeil et les attributs associés.</description>
  <description language="ES">Le permite comprobar el comportamiento del sueño y atributos asociados.</description>
  <quantityMeasurementTypes>
    <quantityMeasurementType id="SLEEP_SLEEPAMOUNT" precision="4" scale="1">
      <quantity id="SLEEPAMOUNT_QTYID" standardUnit="SLEEPAMOUNT_STDUNIT_HR">
        <title language="EN">Sleep Amount</title>
        <title language="FR">Somme du sommeil</title>
        <title language="ES">Cantidad de sueño</title>
        <units>
          <unit id="SLEEPAMOUNT_STDUNIT_HR" />
          <unit id="SLEEPAMOUNT_STDUNIT_MIN" />
        </units>
      </quantity>
      <globalRanges>
        <upperNormalValue value="9">
          <title language="EN">Upper Normal</title>
          <title language="FR">Valeur normale supérieure</title>
          <title language="ES">Valor normal superior</title>
        </upperNormalValue>
        <lowerNormalValue value="6">
          <title language="EN">Lower Normal</title>
          <title language="FR">Valeur normale inférieure</title>
          <title language="ES">Valor normal inferior</title>
        </lowerNormalValue>
        <hypoValue value="2">
          <title language="EN">Hypo</title>
          <title language="FR">Hypo</title>
          <title language="ES">Hipo</title>
        </hypoValue>
      </globalRanges>
    </quantityMeasurementType>
    <quantityMeasurementType id="SLEEP_HEARTRATE" precision="3" scale="0">
      <quantity id="HEARTRATE" standardUnit="HEARTRATE_STDUNIT_BPM">
        <title language="EN">Heart rate</title>
        <title language="FR">Rythme Cardiaque</title>
        <title language="ES">Ritmo Cardiaco</title>
        <units>
          <unit id="HEARTRATE_STDUNIT_BPM" />
        </units>
      </quantity>
      <globalRanges>
        <upperNormalValue value="80">
          <title language="EN">Upper Normal</title>
          <title language="FR">Valeur normale supérieure</title>
          <title language="ES">Valor normal superior</title>
        </upperNormalValue>
        <lowerNormalValue value="60">
          <title language="EN">Lower Normal</title>
          <title language="FR">Valeur normale inférieure</title>
          <title language="ES">Valor normal inferior</title>
        </lowerNormalValue>
        <hyperValue value="100">
          <title language="EN">Hyper</title>
          <title language="FR">Hyper</title>
          <title language="ES">Hiper</title>
        </hyperValue>
        <hypoValue value="50">
          <title language="EN">Hypo</title>
          <title language="FR">Hypo</title>
          <title language="ES">Hipo</title>
        </hypoValue>
      </globalRanges>
    </quantityMeasurementType>
  </quantityMeasurementTypes>
  <categoryMeasurementTypes>
    <categoryMeasurementType id="SLEEP_TYPEOFSLEEP" entriesPerMeasurement="1">
      <title language="EN">Type of Sleep</title>
      <title language="FR">Type de sommeil</title>
      <title language="ES">Tipo de sueño</title>
      <categoryMeasurementItems>
        <categoryMeasurementItem id="TYPEOFSLEEP_VALUE_RESTLESSSLEEP" orderIndex="1">
          <title language="EN">Restless Sleep</title>
          <title language="FR">Sommeil agité</title>
          <title language="ES">Sueño inquieto</title>
        </categoryMeasurementItem>
        <categoryMeasurementItem id="TYPEOFSLEEP_VALUE_GOODSLEEP" orderIndex="2">
          <title language="EN">Good sleep</title>
          <title language="FR">Bien sommeil</title>
          <title language="ES">Buen sueño</title>
        </categoryMeasurementItem>
        <categoryMeasurementItem id="TYPEOFSLEEP_VALUE_DEEPSLEEP" orderIndex="3">
          <title language="EN">Deep Sleep</title>
          <title language="FR">Sommeil profound</title>
          <title language="ES">Sueño profundo</title>
        </categoryMeasurementItem>
      </categoryMeasurementItems>
    </categoryMeasurementType>
    <categoryMeasurementType id="SLEEP_ENVIRONMENT" entriesPerMeasurement="2">
      <title language="EN">Environment</title>
      <title language="FR">Environnement</title>
      <title language="ES">Ambiente</title>
      <categoryMeasurementItems>
        <categoryMeasurementItem id="ENVIRONMENT_VALUE_HUMID" orderIndex="1">
          <title language="EN">Humid</title>
          <title language="FR">Humide</title>
          <title language="ES">Húmedo</title>
        </categoryMeasurementItem>
        <categoryMeasurementItem id="ENVIRONMENT_VALUE_COLD" orderIndex="2">
          <title language="EN">Cold</title>
          <title language="FR">Du froid</title>
          <title language="ES">Frío</title>
        </categoryMeasurementItem>
        <categoryMeasurementItem id="ENVIRONMENT_VALUE_HOT" orderIndex="3">
          <title language="EN">Hot</title>
          <title language="FR">Chaud</title>
          <title language="ES">Caliente</title>
        </categoryMeasurementItem>
        <categoryMeasurementItem id="ENVIRONMENT_VALUE_QUIET" orderIndex="4">
          <title language="EN">Quiet</title>
          <title language="FR">Silencieux</title>
          <title language="ES">Tranquilo</title>
        </categoryMeasurementItem>
        <categoryMeasurementItem id="ENVIRONMENT_VALUE_NOISY" orderIndex="5">
          <title language="EN">Noisy</title>
          <title language="FR">Bruyant</title>
          <title language="ES">Ruidoso</title>
        </categoryMeasurementItem>
      </categoryMeasurementItems>
    </categoryMeasurementType>
  </categoryMeasurementTypes>
</Measurement>
```
The following entry by a patient for a particular day can be captured using this  XML:

* Amount of Sleep: 7 hrs    
*  Average Heart Rate: 80 bpm
*  Type of Sleep: Good
*   Environment: Hot, Humid, Quiet
    
### Understand the XML 
The following section contains XML snippets and the requirements addressed by the snippet:

#### XML Snippet
```
<Measurement id="condition_insomnia">
  <title language="EN">Insomnia</title>
  <title language="FR">Insomnie</title>
  <title language="ES">Insomnio</title>
  <description language="EN">Enables you to check sleep behavior and associated attributes.</description>
  <description language="FR">Permet de vérifier le comportement du sommeil et les attributs associés.</description>
  <description language="ES">Le permite comprobar el comportamiento del sueño y atributos asociados.</description>
``` 
This XML snippet addresses the following requirements:
*   ID and title of the condition
*   Description of the condition
*   Translation of the title and the description

#### XML Snippet  

```
 <quantityMeasurementTypes>
    <quantityMeasurementType id="SLEEP_SLEEPAMOUNT" precision="4" scale="1">
      <quantity id="SLEEPAMOUNT_QTYID" standardUnit="SLEEPAMOUNT_STDUNIT_HR">
        <title language="EN">Sleep Amount</title>
        <title language="FR">Somme du sommeil</title>
        <title language="ES">Cantidad de sueño</title>
        <units>
          <unit id="SLEEPAMOUNT_STDUNIT_HR" />
          <unit id="SLEEPAMOUNT_STDUNIT_MIN" />
        </units>
      </quantity>
      <globalRanges>
        <upperNormalValue value="9">
          <title language="EN">Upper Normal</title>
          <title language="FR">Valeur normale supérieure</title>
          <title language="ES">Valor normal superior</title>
        </upperNormalValue>
        <lowerNormalValue value="6">
          <title language="EN">Lower Normal</title>
          <title language="FR">Valeur normale inférieure</title>
          <title language="ES">Valor normal inferior</title>
        </lowerNormalValue>
        <hypoValue value="2">
          <title language="EN">Hypo</title>
          <title language="FR">Hypo</title>
          <title language="ES">Hipo</title>
        </hypoValue>
      </globalRanges>
    </quantityMeasurementType>
    <quantityMeasurementType id="SLEEP_HEARTRATE" precision="3" scale="0">
      <quantity id="HEARTRATE" standardUnit="HEARTRATE_STDUNIT_BPM">
        <title language="EN">Heart rate</title>
        <title language="FR">Rythme Cardiaque</title>
        <title language="ES">Ritmo Cardiaco</title>
        <units>
          <unit id="HEARTRATE_STDUNIT_BPM" />
        </units>
      </quantity>
      <globalRanges>
        <upperNormalValue value="80">
          <title language="EN">Upper Normal</title>
          <title language="FR">Valeur normale supérieure</title>
          <title language="ES">Valor normal superior</title>
        </upperNormalValue>
        <lowerNormalValue value="60">
          <title language="EN">Lower Normal</title>
          <title language="FR">Valeur normale inférieure</title>
          <title language="ES">Valor normal inferior</title>
        </lowerNormalValue>
        <hyperValue value="100">
          <title language="EN">Hyper</title>
          <title language="FR">Hyper</title>
          <title language="ES">Hiper</title>
        </hyperValue>
        <hypoValue value="50">
          <title language="EN">Hypo</title>
          <title language="FR">Hypo</title>
          <title language="ES">Hipo</title>
        </hypoValue>
      </globalRanges>
    </quantityMeasurementType>
  </quantityMeasurementTypes>
  ```
This XML snippet addresses the numerical task requirements.
#### XML Snippet  
```
 <categoryMeasurementTypes>
    <categoryMeasurementType id="SLEEP_TYPEOFSLEEP" entriesPerMeasurement="1">
      <title language="EN">Type of Sleep</title>
      <title language="FR">Type de sommeil</title>
      <title language="ES">Tipo de sueño</title>
      <categoryMeasurementItems>
        <categoryMeasurementItem id="TYPEOFSLEEP_VALUE_RESTLESSSLEEP" orderIndex="1">
          <title language="EN">Restless Sleep</title>
          <title language="FR">Sommeil agité</title>
          <title language="ES">Sueño inquieto</title>
        </categoryMeasurementItem>
        <categoryMeasurementItem id="TYPEOFSLEEP_VALUE_GOODSLEEP" orderIndex="2">
          <title language="EN">Good sleep</title>
          <title language="FR">Bien sommeil</title>
          <title language="ES">Buen sueño</title>
        </categoryMeasurementItem>
        <categoryMeasurementItem id="TYPEOFSLEEP_VALUE_DEEPSLEEP" orderIndex="3">
          <title language="EN">Deep Sleep</title>
          <title language="FR">Sommeil profound</title>
          <title language="ES">Sueño profundo</title>
        </categoryMeasurementItem>
      </categoryMeasurementItems>
    </categoryMeasurementType>
    <categoryMeasurementType id="SLEEP_ENVIRONMENT" entriesPerMeasurement="2">
      <title language="EN">Environment</title>
      <title language="FR">Environnement</title>
      <title language="ES">Ambiente</title>
      <categoryMeasurementItems>
        <categoryMeasurementItem id="ENVIRONMENT_VALUE_HUMID" orderIndex="1">
          <title language="EN">Humid</title>
          <title language="FR">Humide</title>
          <title language="ES">Húmedo</title>
        </categoryMeasurementItem>
        <categoryMeasurementItem id="ENVIRONMENT_VALUE_COLD" orderIndex="2">
          <title language="EN">Cold</title>
          <title language="FR">Du froid</title>
          <title language="ES">Frío</title>
        </categoryMeasurementItem>
        <categoryMeasurementItem id="ENVIRONMENT_VALUE_HOT" orderIndex="3">
          <title language="EN">Hot</title>
          <title language="FR">Chaud</title>
          <title language="ES">Caliente</title>
        </categoryMeasurementItem>
        <categoryMeasurementItem id="ENVIRONMENT_VALUE_QUIET" orderIndex="4">
          <title language="EN">Quiet</title>
          <title language="FR">Silencieux</title>
          <title language="ES">Tranquilo</title>
        </categoryMeasurementItem>
        <categoryMeasurementItem id="ENVIRONMENT_VALUE_NOISY" orderIndex="5">
          <title language="EN">Noisy</title>
          <title language="FR">Bruyant</title>
          <title language="ES">Ruidoso</title>
        </categoryMeasurementItem>
      </categoryMeasurementItems>
    </categoryMeasurementType>
  </categoryMeasurementTypes>
  ```
 This XML snippet addresses the fixed value task requirements.

## Specify Units of Measure

You can define units of measure using the [`POST /UnitofMeasure` endpoint](https://nikhil-mukunda.github.io/APIDocs/#/Specify%20Units%20of%20Measure/UnitsofMeasure).

### Example
You can define the following units of measure:

|Id|Language Key|Title|
|---|---|---|
|SLEEPAMOUNT_STDUNIT_HR|EN|Hours|
|SLEEPAMOUNT_STDUNIT_HR|FR|Heures|
|SLEEPAMOUNT_STDUNIT_HR|ES|Horas|
|SLEEPAMOUNT_STDUNIT_MIN|EN|Minutes|
|SLEEPAMOUNT_STDUNIT_MIN|FR|Minutes|
|SLEEPAMOUNT_STDUNIT_MIN|ES|Minutos|
|HEARTRATE_STDUNIT_BPM|EN|bpm|
|HEARTRATE_STDUNIT_BPM|FR|bpm|
|HEARTRATE_STDUNIT_BPM|ES|bpm|

## Stage and Activate Tasks
After you create your XML, you perform the following actions:
1. Stage the XML on the SAP Health for Patient Engagement platform. See the [`PUT /MeasurementStagings` endpoint](https://nikhil-mukunda.github.io/APIDocs/#/Add%20your%20tasks%20to%20staging%20area/StageMeasurement).
2. Activate the tasks so that the patient can use these tasks. See the [`PUT /ActivateMeasurement` endpoint](https://nikhil-mukunda.github.io/APIDocs/#/Activate%20the%20tasks/ActivateMeasurement). 

After you implement this logic in the mobile app of the patient, the user interface for the tasks you have defined is automatically created for the physician.
The physician can then monitor the patient's health as the patient performs tasks. 
