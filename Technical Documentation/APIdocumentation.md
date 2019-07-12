Singapore Academy of Law  
LawNet Legal Research API
<br>
Developer’s Guide  
Version 1.0 (For Marketing Purposes)
<br>
<br>

## RELEASE NOTES
|  Version  |   Release Date   |    Remarks    |
|-----------|------------------|---------------|
|    1.0    |  1 Feburary 2019 | Initial Draft |

<br>

## TABLE OF CONTENTS
1. [INTRODUCTION](#itrd) <br>

1. [LIBRARIES](#lbries) <br>

    2.1 cURL <br>
    2.2 C# <br>
    2.3 JAVA <br>
    2.4 POSTMAN <br>

1. [API REFERENCE](#apiref) <br>

    3.1 INTRODUCTION <br>
        &nbsp; &nbsp; &nbsp; 3.1.1 Base URL <br>
        &nbsp; &nbsp; &nbsp; 3.1.2 Authentication <br>
        &nbsp; &nbsp; &nbsp; 3.1.3 Data Access Level (Entitlement) <br>
        &nbsp; &nbsp; &nbsp; 3.1.4 Response Codes <br>
   
     3.2 LEGAL RESEARCH APIS <br>
         &nbsp; &nbsp; &nbsp; 3.2.1 Searching <br>
         &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 3.2.1.1 Sample Requests <br>
         &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 3.2.1.2 Sample Response <br>
         &nbsp; &nbsp; &nbsp; 3.2.2 Retrieving Content <br>
         &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 3.2.2.1 Sample Requests <br>
         &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 3.2.2.2 Sample Response <br>
         
1. [FAQ](#qn)

----
<br>

## 1 INTRODUCTION <a name="itrd"></a>

This document aims to provide an overview of the Singapore Academy of Law’s (“SAL”) LawNet Legal Research APIs and how you can best use them to create new products and services to enhance your applications with legal data. SAL’s Legal Research APIs provide the ability to:

•	Search Cases in LawNet <br>
•	Get Full Text of Cases in LawNet

To use our Legal Research APIs, an API Key and data entitlement will be created for you by Singapore Academy of Law to access the API end points. Application forms are available and access will be subject to all relevant Terms and Conditions.
Where appropriate, access will only be granted to a sandbox environment which comprises of a subset of the production content. This would enable you to assess the APIs and data to see how they can integrate with your systems.


## 2 LIBRARIES <a name="lbries"></a>

Our Legal Research APIs are built on HTTP. Our API is RESTful and it:

- Uses predictable, resource-oriented URLs.
- Uses built-in HTTP capabilities for passing parameters and authentication.
- Responds with standard HTTP response codes to indicate errors.
- Returns XML.

You may use your favorite or any suggested HTTP/REST library available for your programming language, to make
HTTP calls to our Legal Research APIs.

Below are some common language-specific notes which you may use.

>### 2.1 cURL
>[cURL](http://linux.die.net/man/1/curl) is a popular command line tool to make HTTP requests with. It is a very simple but quite powerful tool. With it, you can send data using any HTTP method. You can send post data and query parameters and files in a very consistent and elegant way.

>### 2.2 C#
>For C# developers, you may use [RestSharp](http://restsharp.org/).

>### 2.3 JAVA
>Check out the [UniRest](http://unirest.io/java.html) REST client if Java is your weapon of choice. You will also need the following dependencies, eg. org.json, httpclient 4.3.6, httpmime 4.3.6, httpasyncclient 4.0.2

>### 2.4 POSTMAN
>[Postman](https://www.getpostman.com/) is a Chrome add-on and Mac application which is used to fire requests to an API. It is very lightweight and fast. Requests can be organised in groups, and tests can also be created with verifications for certain
conditions on the response. It is possible to make different kinds of HTTP requests – GET, POST, PUT, PATCH and
DELETE. It is also possible to add headers to the requests. 

## 3 API REFERENCE <a name="apiref"></a>

### 3.1 INTRODUCTION

This section provides you with information about the Legal Research APIs and sample scripts (refer to section 3.2)
that will function. You are welcome to copy/paste and run the scripts to see the APIs in action.

#### 3.1.1	Base URL
All Legal Research API URLs referenced in this documentation start with the following base part:

Trial:
```https://test-legalresearch.api.sal.sg```

#### 3.1.2 Authentication

When you sign up for an account and approval is granted, you will be given a set of Access Credentials including
an APIkey. The APIkey will determine the content available to your account.

Authentication with the Legal Research APIs is done by inserting the APIkey as a parameter and as part of header
in the request.

Authentication with the API gateway occurs when searching for cases and retrieving content.

_```Important: Keep your API key secret!```_

#### 3.1.3 Data Access Level (Entitlement)

In addition to the APIkey, an excel sheet containing a list of content access codes will also be provided to you for
accessing of data. The access codes for the content is required as a parameter when using services in sections
3.2.1 and 3.2.2.

<img src="/Technical Documentation/Data Access Level (Entitlement).png" alt="Data Access Level"/>

#### 3.1.4 Response Codes

The following standard HTTP response codes.

| Code |Description|
|----------------|-------------------------------------------------|
|200| Success - search results or full text of content returned.|
|400| Bad Request - Often this is because a required parameter is missing or a parameter’s value is invalid. Note: this is returned in XML format.|
|402| Request Failed - Parameters valid but request failed|
|403| Forbidden – Invalid/Missing API Key|
|404| Not Found - The requested item doesn’t exist|
|500, 502, 503, 504| Server Errors - something is wrong on our end|

The table below provides you with various scenarios and the expected responses, based on an entitlement below.

|Scenario|Description|Response Code|Category Code|Level 2 Category Code|Level 3 Category Code|Expected Response (xml)|
|------|------|------|------|------|------|-------------------------------------------------|
1|Category Code is empty|400||r1c1| r1c1sc1|```<response><status>error</status><message>Category is a mandatory parameter. Please select a valid category and try again. </message></response>```
2|Level 2 category code is empty|400|r1|| r1c1sc1| ```<response><status>error</status><message>Level 2 category is a mandatory parameter. Please add a valid sub category and try again. </message></response>```
3|Level 3 category code is empty|400|r1| r1c1 ||```<response><status>error</status><message>Level 3 category is a mandatory parameter. Please add a valid level 3 category and try again. </message></response>```
4.1|Level 1 to 3 category codes provided are correct|200|r1| r1c1| r1c1sc1 (To retrieve specific content)|Success: Results will be displayed. Refer to sections 3.2.1.2 and 3.2.2.2
4.2|Level 1 to 3 category codes provided are correct|200|r1| r1c2| #r1c2 (To retrieve Judgement collection)|Success: Results will be displayed. Refer to sections 3.2.1.2 and 3.2.2.2
4.3|Level 1 to 3 category codes provided are correct|200|r1, r3| r1c1, r3c14| r1c1sc1, r3c14sc1|Success: Results will be displayed. Refer to sections 3.2.1.2 and 3.2.2.2
5|Invalid Category code or category code that you are not entitled to access|400|r1, r2|r1c1|r1c1sc1|```<response><status>error</status><message>The category requested [r2] is invalid. The level 2 category requested [r1c2,r1c3] is invalid. The level 3 category requested [#r1c2, #r1c3] is invalid</message></response>```
6|Level 3 category code that is not available in Legal Research|400|r1|r1c1|r1c1sc50|```<response><status>error</status><message>No matching Level 3 category found for the level 2 category Judgements:r1c1. Please add a valid sub category and try again. The level 3 category requested [r1c1sc50] is invalid </message></response>```

### 3.2 LEGAL RESEARCH APIS

#### 3.2.1 Searching

This API allows you to search for cases within the LawNet Legal Research module.

```POST /v1-search/search```

The following basic POST query parameters allow you to specify the cases you want:

|Field name|Description|Mandatory|Data type|
|----------|------------|------|------|
apikey| API Key allocated by SAL|Yes|String
cats| List of Content Groups. For multiple, please use comma separated values.|Yes|String
l2cats| List of Content Categories. For multiple, please use comma separated values.|Yes|String
l3cats| List of Content Subcategories. For multiple, please use comma separated values.|Yes| String
searchTerm|Search Term|Yes|String
page|Current page number. [Default :1]|No|Integer
maxperpage|Max per page in pagination [Default :20]|No|Integer
surroundingWords| Surrounding Words [Default :10]| No| Integer
orderBy| Ordering of search result using following value [title-asc, title-des, date-asc, date-des, relevance] [Default: relevance]|No|String

By default, the response will be returned in XML form with parsed parts.

These are the elements of the XML returned from a POST request to search cases.

|Elements|Description|Type|
|------|-------------------------|----------|
searchResponse| Root Node of Search Results.| Single
searchStatistic| Child node of searchResponse, contain the search result stats eg. Total, Pagination count.|Single
total|Child node of searchStatistic, contain total number of search results returned|Single
paginationCount| Child node of searchStatistic, contain total number of pagination|Single
searchResults| Child node of searchResponse| Single
resultList| Child node of searchResults. This node holds multiple result of document content containing the search term.|Single
result| Child node of resultList, contain information on each search items|Multiple
documentId| Child node of result. This node holds the Document URL. Note: The value in this node will be used for retrieving content.|Single
document| Child node of result. This node holds title and citation details.|Single
Title| Child node of document. Title of the document.| Single
Citation|Child node of document. Citation of the document| Single
Format| Child node of result. Indication of document formats available (PDF and XML) – Yes/No.|Multiple
relevance| Child node of result. Relevance score of the document|Single
category| Child node of result. Category assigned to the document|Single
documentTooltipHolder| Child node of result. This node hold list of document metadata.|Multiple
casereference| Child node of result. Indicates the precedential value of the document according to the annotations.|Single
following| Child node of casereference. This is used to denote that the principle of law established in the instant case (or the dictum referred to) has been applied in subsequent cases.|Single
referring|Child node of casereference.This is used to describe cases which make only a general reference to the instant case.|Single
distinguishing| Child node of casereference.This is used where the instant case is not applied in subsequent cases due to some distinction in the facts or in the law.|Single
notfollowing| Child node of casereference.This is used where the court in subsequent cases has consciously refused to follow the instant case although potentially relevant. It implies that the instant case is wrong.|Single
overruling| Child node of casereference. This is used only where a higher court with the power to overrule has held the instant case to be wrong.|Single
snippets|Child node of result. This node holds multiple snippets of document content containing the search term.|Single
snippet| Child node of snippets. A snippet of document content containing the search term|Multiple

### 3.2.1.1 Sample Requests

**_3.2.1.1.1 cURL_**

```
curl -X POST \

  https://test-legalresearch.api.sal.sg/v1-content/content \
  
  -H 'Content-Type: application/x-www-form-urlencoded' \
  
  -H 'cache-control: no-cache' \
  
  -H 'x-api-key: {{API KEY}}' \
  
  -d 'apikey={{API KEY}}&cats=r1&l2cats=r1c1%2Cr1c2%2Cr1c3&l3cats=%23r1c1%2C%23r1c2%2C%23r1c3&docUrl=%2Fsample group%2Fsample 1.xml&format=xml'
    
```

**_3.2.1.1.2 C# (RestSharp)_**

```
var client = new RestClient("https://test-legalresearch.api.sal.sg/v1-search/search");

var request = new RestRequest(Method.POST);

request.AddHeader("cache-control", "no-cache");

request.AddHeader("x-api-key", "{{API KEY}}");

request.AddHeader("Content-Type", "application/x-www-form-urlencoded");

request.AddParameter("undefined", “apikey={{API KEY}}&cats=r1&l2cats=r1c1%2Cr1c2%2Cr1c3&l3cats=%23r1c1%2C%23r1c2%2C%23r1c3&searchTerm=sample&page=1&maxperpage=15&orderBy=date-des&surroundingWords=5”, ParameterType.RequestBody);

IRestResponse response = client.Execute(request);

```

**_3.2.1.1.3 Java (Unirest)_**

```
HttpResponse<String> response = Unirest.post("https://test-legalresearch.api.sal.sg/v1-search/search")

  .header("Content-Type", "application/x-www-form-urlencoded")
  
  .header("x-api-key", "{{API KEY}}")
  
  .header("cache-control", "no-cache")
  
  .body("apikey={{API KEY}}&cats=r1&l2cats=r1c1%2Cr1c2%2Cr1c3&l3cats=%23r1c1%2C%23r1c2%2C%23r1c3&searchTerm=sample&page=1&maxperpage=15&orderBy=date-des&surroundingWords=5")
  
  .asString();

```
### 3.2.1.2 Sample Response

```<searchResponse>
    <searchStatistic>
        <total>100</total>
    </searchStatistic>
    <searchResults>
        <resultList>
            <result>
                <documentId>/Sample Group/sample 1.xml</documentId>
                <document>
                    <Title>
      Sample Title 1</Title>
                    <Citation>[2019] sample 1</Citation>
                </document>
                <format>
                    <xml>Yes</xml>
                    <pdf>No</pdf>
                </format>
                <relevance>34</relevance>
                <category>Sample Group</category>
                <documentTooltipHolder>
                    <court>Court</court>
                    <corams>Luke Tan</corams>
                    <date>18 April 2018</date>
                    <caseno>sample-1111-2019 &amp; Ors</caseno>
                    <catchword>No catchword</catchword>
                </documentTooltipHolder>
                <casereference>
                    <following>0</following>
                    <referring>6</referring>
                    <distinguishing>0</distinguishing>
                    <notfollowing>0</notfollowing>
                    <overruling>0</overruling>
                </casereference>
                <snippets>
                    <snippet>...carefully laid trap by the 
                        <b>sample</b> content to...
                    </snippet>
                </snippets>
            </result>
            <result>
                <documentId>/Sample Group/sample 2.xml</documentId>
                <document>
                    <Title>
      Sample Title 2</Title>
                    <Citation>[2017] Sample 2</Citation>
                </document>
                <format>
                    <xml>Yes</xml>
                    <pdf>No</pdf>
                </format>
                <relevance>58</relevance>
                <category>Sample Group</category>
                <documentTooltipHolder>
                    <court>Court</court>
                    <corams>Lim David</corams>
                    <date>18 December 2017</date>
                    <caseno>Sample 12344/2019, Sample No.1111/2019/01</caseno>
                    <catchword>Damages,  Tort</catchword>
                </documentTooltipHolder>
                <casereference>
                    <following>0</following>
                    <referring>0</referring>
                    <distinguishing>0</distinguishing>
                    <notfollowing>0</notfollowing>
                    <overruling>0</overruling>
                </casereference>
                <snippets>
                    <snippet>shouting “protest against the 
                        <b>sample</b> content!”; and
                    </snippet>
                    <snippet>protest against the 
                        <b>sample</b> content
                    </snippet>
                </snippets>
            </result>
        </resultList>
    </searchResults>
</searchResponse>
```

### 3.2.2 Retrieving Content

This API allows you to retrieve content within the LawNet Legal Research.

```POST /v1-content/content```

The following basic POST query parameters allow you to specify the cases you want:

|Field name|Description|Mandatory|Data type
|------|-------------------------|----|----------|
apikey| API Key Allocated|Yes|String
cats| List of Content Groups. For multiple. Comma separated values.|Yes|String
l2cats| List of Content categories. For multiple. Comma separated values.|Yes|String
l3cats|List of Content subcategories. For multiple. Comma separated values.|Yes|String
docUrl|Document URL is only available in the Search Response. Example: ```<documentId>/Sample Group/sample1.xml</documentId>)```|Yes|String
Format|Format of content. Note: The format XML or PDF will be used to retrieve the entitled document content.|No|String

By default, the response will be returned in XML form with parsed parts.

These are the elements of the XML returned from a POST request to get case content.

|Elements|Description|Type|
|------|-------------------------|----------|
searchResponse| Root node of content. | Single
pdf| Child node of searchResponse. This node holds details of a PDF.| Single
file| Child node of pdf. This node holds the file name of PDF.| Single
supportingFiles|Child node of searchResponse. This node holds multiple supporting files. (Example: images)|Single
supporting| Child node of supprtingFiles. This node holds filename and binary.|Multiple
image|Child node of supporting. This node holds name of the file.|Single
title|Child node of image. Contains name of the file.|Single
binary|Child node of image/pdf. Contains binary format of the images or PDFs. Note: developers will be required to convert to physical files programmatically.||
Copyright|Child node of searchResponse. This value holds the copyright statement.|Single
mainContent|Child node of searchResponse. This node contains body content of the document. (XML/HTML+CSS)|Single

### 3.2.2.1 Sample Requests

**_3.2.2.1.1 cURL_**

```
curl -X POST \

  https://test-legalresearch.api.sal.sg/v1-content/content \
  
  -H 'Content-Type: application/x-www-form-urlencoded' \
  
  -H 'cache-control: no-cache' \
  
  -H 'x-api-key: {{API KEY}}' \
  
  -d 'apikey={{API KEY}}&cats=r1&l2cats=r1c1%2Cr1c2%2Cr1c3&l3cats=%23r1c1%2C%23r1c2%2C%23r1c3&docUrl=%2Fsample group%2Fsample 1.xml&format=xml'

```
**_3.2.2.1.2 C# (RestSharp)_**

```

var client = new RestClient("https://test-legalresearch.api.sal.sg/v1-content/content");
var request = new RestRequest(Method.POST);
request.AddHeader("cache-control", "no-cache");
request.AddHeader("x-api-key", "{{API KEY}}");
request.AddHeader("Content-Type", "application/x-www-form-urlencoded");
request.AddParameter("undefined",”apikey={{API KEY}}&cats=r1&l2cats=r1c1%2Cr1c2%2Cr1c3&l3cats=%23r1c1%2C%23r1c2%2C%23r1c3&docUrl=%2Fsample group%2Fsample 1.xml&format=xml”, ParameterType.RequestBody);
IRestResponse response = client.Execute(request);

```

**_3.2.2.1.3 Java (Unirest)_**

```
HttpResponse<String> response = Unirest.post("https://test-legalresearch.api.sal.sg/v1-content/content")
  .header("Content-Type", "application/x-www-form-urlencoded")
  .header("x-api-key", "{{API KEY}}")
  .header("cache-control", "no-cache")
  .body(“apikey={{API KEY}}&cats=r1&l2cats=r1c1%2Cr1c2%2Cr1c3&l3cats=%23r1c1%2C%23r1c2%2C%23r1c3&docUrl=%2Fsample group%2Fsample 1.xml&format=xml)
  .asString();

```
**_3.2.2.2 Sample Response_**

&nbsp; &nbsp; &nbsp; &nbsp; 3.2.2.2.1 [XML](https://github.com/legaltechsal/LawNet-APIs/blob/master/Technical%20Documentation/Sample%20Response%20-%20XML.xml)

&nbsp; &nbsp; &nbsp; &nbsp; 3.2.2.2.2 [XML+Images](https://github.com/legaltechsal/LawNet-APIs/blob/master/Technical%20Documentation/Sample%20Response%20-%20XML%20%2B%20Images.xml)

&nbsp; &nbsp; &nbsp; &nbsp; 3.2.2.2.2 [HTML+CSS+Supporting Documents](https://github.com/legaltechsal/LawNet-APIs/blob/master/Technical%20Documentation/Sample%20Response%20-%20HTML%20%2B%20CSS%20%2B%20Supporting%20Documents.xml)
<br>

## 4 FAQ <a name="qn"></a>

<br>

Q: Why I am receiving response “forbidden”?
<br>
A: This is because the API key used is invalid or empty or not found in header under key x-api-key. <br>
{ "message": "Forbidden"}.

<br>

Q: I’m not able to convert binary PDF and image to physical file. How can I do it?
<br>
A: You need to convert the binary String to Byte Array and simply write the bytes onto your local machine
programmatically.
<br>
Sample code in C#:
```
byte[] bytes;
bytes = StringToByteArray(binaryString);
System.IO.File.WriteAllBytes(@"C:\Projects\hello.pdf", bytes);
```
<br>

Q: What is the API request limit set to my account?
<br>
A: During the 90-day trial access period, the number of API calls are capped at 5 calls per second. 

<br>

Q: How do I report an issue encountered?
<br>
A: Please provide the details on

1. API endpoint
1. Headers
1. API request
1. response/error

Sample screenshot from Postman
<img src="/Technical Documentation/Github - Postman sample screenshot.JPG" alt="Postman Sample Screenshot"/>
<br>

Q: What are the search operators available?
<br>
A: <br>

Search Operator|Explanation|Examples
|:------:|:-------------------------:|:----------:|
AND|Performs "all of the words" search, i.e. retrieves documents that contain all the entered keywords. <br><br> If multiple words are entered without using any operators, the AND operator is applied by default.|negligence AND causation <br><br> negligence causation 
OR| Performs "any of the words" search, i.e. retrieves documents that contain any of the entered keywords|forfeiture OR eviction
" "|Performs an "exact phrase" search, i.e. retrieves documents containing the exact phrase within the quotation marks.|"passing off"
-|Functions as a NOT operator, i.e. excludes documents that contain the entered keyword|-malaysia <br><br>agent -insurance
NEAR/# <br> NEAR|Functions as a proximity operator, i.e. retrieves documents that contain the entered keywords with # number of words between them.<br> (If the NEAR operator is used without specifying the /#number, the default will be 10 words.)|breach NEAR/3 contract <br><br> harassment <br> NEAR act
*|Multi-character wildcard operator used to replace any number of characters in a word to search for variations of the word|appli* <br><br> Moham*d
?|Single character wildcard operator used to replace a single character in a word to search for spelling variations|debt?r <br><br> organi?ation
NOT_IN|"not in" operator, i.e. retrieves documents containing the keyword but excludes documents containing the keyword in a phrase/context.|minority <br> NOT_IN <br> "minority shareholder"
( )|Grouping operator, used to group together search terms which should be executed together|(doctor OR surgeon) AND negligence <br><br>"specific performance" OR (damages NEAR/5 contract)^

