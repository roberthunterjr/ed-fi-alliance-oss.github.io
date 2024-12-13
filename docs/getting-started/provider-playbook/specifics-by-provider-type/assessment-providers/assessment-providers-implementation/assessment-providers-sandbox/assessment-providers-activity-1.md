# Assessment Providers - Activity 1

:::info Key Concepts

* The Ed-Fi API Sandbox is a quick and easy way to explore how an actual API exchange works.
* HTTP GET operations are used to query the API and get data back.

:::

## Activity 1 - Get all Assessments in the Sandbox

In this activity, we will get a record of assessments in the API (in the Ed-Fi data model, an **assessment** is a record of an assessment instrument that students are assessed on). As such, the data we get back will not contain student performance data. Rather, these records tell us what assessments we can use to lookup student results. (More about the data model is presented in the [next section](https://edfi.atlassian.net/wiki/display/TT/3.1.4+Assessment+Providers+-+Data+Model).)

1. Open a separate browser window and go to [https://api.ed-fi.org](https://api.ed-fi.org/). This is the public Ed-Fi ODS API sandbox. It lists all versions of the ODS implementation that are currently under support.
2. Click on the "Documentation & Sandbox" link next to "Ed-Fi ODS / API v2.5"
3. On the next page, click on the "Resources" link. This shows the full ODS API, which is massive. However, be aware that only a small number of these APIs are related to assessment.
4. In the top right corner, click on the "Authorize" button, and click on the "Authorize" button on the popup window. This authorizes your browser to communicate with this API.
5. Scroll down to the "assessments" API resource and click on it. This opens up documentation of all methods available on the /assessments API resource
6. Click on the first "GET" in the list that appears. This expands the info on the GET operation. In it, you can see an example of the JSON object to expect when you do a GET.
7. Scroll down and click the "Try it Out Button" - this requests that the API send all /assessment resources back to your browser.
8. You will see info that looks like this appear.

 ![Assessment Query Screenshot](https://edfi.atlassian.net/wiki/download/thumbnails/22905175/assessment_query.png?version=1&modificationDate=1562948657390&cacheVersion=1&api=v2&width=886&height=912)

### Congratulations, you just completed your first assessment data exchange using Ed-Fi

If you received a different response, check the response code and response body for these issues

| Response Code | Response Body | Issue |
| --- | --- | --- |
| 400 | "message": "The argument 'request' is missing or invalid." | The API is complaining that it can't parse the JSON. Check that the JSON is well-formed. There may be a missing brace or bracket. |
| 403 | "message": "No API key information was available for authorization." | The access token you received in step 4 has expired. Go back and redo step 4. |

Read on to learn a bit about the transaction.

### What you sent

In this activity, your browser sent a request to the API using an HTTP GET operation. That transaction was sent a URL that looks like this:

```text
https://api.ed-fi.org:443/v2.5.0/api/api/v2.0/2019/assessments
```

Further, that request included the access token (a "Bearer" token) you created earlier, sent in the header of the HTTP request. You can see that in the Web page under the "CURL" section, which will look like this:

```text
--header 'Authorization: Bearer [some long string of letters and numbers]'
```

You did this via the HTTP GET method.

### What you received

You got back a few different things, all in an HTTP response package. What you got back:

1. A response code
2. Response headers
3. A response body

The **response code** (or more accurately, the response status code) is from a [standardized list of HTTP response codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) and tells you what the outcome was. Hopefully you received a "200", which means success.

The **response headers** were the headers on the server's response back to you. They provide information that might be of interest to you, but in this case you can mostly ignore them. In other cases, they can contain important info.

The **response body** is the main event here! This is the data you wanted: info on the assessments available. There is a lot to look at here, but we will focus on a few items only.

* This item is formatted in [JSON](https://en.wikipedia.org/wiki/JSON), a simple yet common language for formatting data objects. Note that a full discussion of JSON is beyond the scope of this tutorial.
* Note that the response is a JSON array. You can tell this by the JSON beginning with a bracket "\[" and ending with one. You received a collection because you asked the API for **all** assessments available.
* Within the array, each assessment is available as a JSON object denoted by curly braces "{" and "}", separated by commas.
* Each assessment JSON has a number of fields and sub-objects as part of it.
* Finally, note that **you may not have received all assessments**. APIs commonly limit the number of items returned so that the client is not overwhelmed. In this case, you probably received 25 items, the API's default. To get more items, you would make a new request for the next 25 items.