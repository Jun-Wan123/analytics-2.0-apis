---
title: Analytics Classification APIs
description: Use Analytics classification APIs to categorize variable data.
---

# Analytics Classification API

The Analytics 2.0 Classification API endpoints allow you to categorize variable data, then display it in different ways when you generate reports. The endpoints use the same data and methods that are used when working with classifications in the UI. See the [Classifications overview](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html) for more information.

The endpoints described in this guide are routed through analytics.adobe.io. To use them, you will need to first create a client with access to the Adobe Developer Console. For more information, refer to [Getting started with the Analytics API](src/pages/guides/endpoints/classifications/index.md).

Additionally, using these endpoints requires your global company ID in each request. You can find your global company ID by using the [Discovery API](../discovery.md)

This guide includes instructions for using the following endpoints:

* POST import JSON classification: Creates a classification from a JSON payload
* POST export classification: Creates an export job for a specified dataset ID
* GET export classification file: Retrieves the output of an export job
* GET classification template: Retrieves a template showing structure of exported data
* GET all classification datasets: Retrieves all datasets for a specified report suite ID
* GET a single classification: Retrieves information for the specified dataset
* GET classification job information: Retrieves job information for the specified job ID
* PUT classification dataset update: Updates a classification dataset
* DELETE classification: Deletes a specified classification

## Importing Classification Datasets

This guide includes instructions for importing JSON classification datasets smaller than 50 MB. With this method, you include the dataset in the payload as part of a POST request. To import classification datasets that are larger than 50 MB, or that include a .tsv or .tab file, see [Importing classifications by file upload](classifications/import-file.md).

## POST import JSON classification

Use this endpoint to create a classification smaller than 50 MB. For more information on importing classifications, see [Classifications importer overview](https://experienceleague.adobe.com/docs/analytics/components/classifications/classifications-importer/c-working-with-saint.html).

`POST https://analytics.adobe.io.api/{GLOBAL_COMPANY_ID}/classification/job/import/json/{DATASET_ID}`

### Request and Response Examples

Request and response examples
Click the **Request** tab in the following example to see a cURL request for this endpoint. Click the **Response** tab to see a successful JSON response for the request.

<CodeBlock slots="heading, code" repeat="2" languages="CURL,JSON"/>

#### Request

```sh
curl -X POST "https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/job/import/json/{DATASET_ID}" \
     -H "x-api-key: {CLIENT_ID}" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer {ACCESS_TOKEN}" \
     -d '{
            "dataFormat": "json",
            "encoding": "UTF8",
            "jobName": "example_dataset_name-example-dataset_id at example_time",
            "notifications": [
                {
                "method": "email",
                "state": "completed",
                "recipients": [
                    "john@example.com
                ]
                }
            ],
            "listDelimiter": ",",
            "source": "Direct API Upload",
            "keyOptions": {
                "byte_length": 0,
                "type": "string"
            },
            "data": [
                {
                "key": "Key20230730-json1",
                "data": {
                    "Product Brand": "Basket Ball Jam",
                    "Category": "",
                    "Size": "Winter Fun",
                    "Weight": "Sports",
                    "Origin": "Origin-4"
                }
                },
                {
                "key": "Key20230730-json2",
                "data": {
                    "Product Brand": "Basket Ball Jam",
                    "Category": "",
                    "Size": "Winter Fun",
                    "Weight": "Sports",
                    "Origin": "Origin-5"
                }
                },
                {
                "key": "Key20230730-json3",
                "data": {
                    "Product Brand": "Basket Ball Jam",
                    "Category": "",
                    "Size": "Winter Fun",
                    "Weight": "Sports",
                    "Origin": "Origin-6"
                }
               }
             ]
            }'
```

#### Response

```json
{
  "import_job_id": "91f38377-b674-4230-9459-e2219cae3e9c",
  "api_job_id": "5e116b45-32a8-4978-b123-4b0ce0eceab1"
}
```

### Request example details

The example above requests the following:

* the `dataFormat` for the classification as `json`
* the `jobName` as `example_dataset_name-example-dataset_id at example_time`
* the `notifications` to be sent by `email` when the status is `completed`
* the import `data` is comma delimited with key value pairs for `key`, `data`, `Category`, `Weight`, and `Origin`

### Response example details

The response example above shows `import_job_id` and ``api_job_id` values that can be used with other classification endpoints.

### Request parameters

The following table describes the POST import JSON classification request parameters:

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| `dataset_id` | required | string | The dataset ID for creating API import job |
| `dataFormat` | optional | string | The data format options |
| `encoding` | optional | string | The encoding for data. The default value is `UTF-8`. |
| `jobName` | optional | string | The name of the job |
| `fileBasename` | optional | string | The name of the file currently being read, without path or extension |
| `notifications` | optional | container | Contains the notification information. Includes the `method`, `state`, and `recipients` parameters. |
| `method` | optional | string | The method by which the notification is sent. This includes the enums `email` and `rabbit`. |
| `state` | optional | string | The state of the notification. Includes the following enums: `created`, `queued`, `validated`, `failed_validation`, `processing`, `done_processing`, `failed_processing`, and `completed`. |
| `recipients` | optional | string | The recipients of the notification |
| `statesWithQueuedNotifications` | optional | string |  |
| `listDelimiter` | optional | string | Specifies the data delimiter for the list. Default delimiter is `,` (comma) |
| `pipelineTag` | optional | string |  |
| `source` | optional | string | The data source. Default value is `"Direct API Upload"`. |
| `dataUri` | optional | string | The data URI |
| `originalDataUri` | optional | string | The original data URI |
| `keyOptions` | optional | container | Contains the `byte_length`, `type`, and `overwrite` parameters |
| `byte_length` | optional | integer | The byte length of the job |
| `type` | optional | string | The type of the job |
| `overwrite` | optional | boolean | Whether or not the import will overwrite. |
| `data` | required |  | The data to be imported |
| `jobImportOption` | optional | container | Contains the `dataFormat`, `encoding`, `jobName`, `fileBasename`, `notifications`, `statesWithQueuedNotifications`,`listDelimeter`, `pipelineTag`, `source`, `dataUri`, `originalDataUri`, `keyOptions`, and `notification_extras` parameters. As described in the following 12 rows. |
| `dataFormat` | optional | string | The data format options. Includes `tsv`, `tab`, or `json`. |
| `encoding` | optional | string | The encoding for data. The default value is `UTF-8`. |
| `jobName` | optional | string | The name of the job |
| `fileBasename` | optional | string | The name of the file currently being read, without path or extension |
| `notifications` | optional | container | Contains the notification information. Includes the `method`, `state`, and `recipients` parameters. |
| `method` | optional | string | The method by which the notification is sent. This includes the enums `email` and `rabbit`. |
| `state` | optional | string | The state of the notification. Includes the following enums: `created`, `queued`, `validated`, `failed_validation`, `processing`, `done_processing`, `failed_processing`, and `completed`. |
| `recipients` | optional | string | The recipients of the notification |
| `statesWithQueuedNotifications` | optional | string |  |
| `listDelimiter` | optional | string | Specifies the data delimiter for the list. Default delimiter is `,` (comma) |
| `pipelineTag` | optional | string |  |
| `source` | optional | string | The data source. Default value is `"Direct API Upload"`. |
| `dataUri` | optional | string | The data URI |
| `originalDataUri` | optional | string | The original data URI |
| `overwrite` | optional | boolean | Whether or not the import will overwrite. |
| `notification_extras` | optional | container | Extra options for notifications. Contains the `key`, and `value` parameters. |
| `key` | optional | string | The field or column name associated with key value |
| `value` | optional | string | The actual value of the key (as in a field or column name) |

### Response Parameters

No response parameters are returned. Successful requests are indicated by a `200` status code.

## POST export classification

Use this endpoint to create an export job for a classification dataset. The dataset can be retrieved in a subsequent request by providing the job ID returned with this endpoint.

`POST https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/job/export/{DATASET_ID}`

### Request and Response Examples

Request and response examples
Click the **Request** tab in the following example to see a cURL request for this endpoint. Click the **Response** tab to see a successful JSON response for the request.

<CodeBlock slots="heading, code" repeat="2" languages="CURL,JSON"/>

#### Request

```sh
curl -X POST 'https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/job/export/{DATASET_ID}' \
     -H "x-api-key: {CLIENT_ID}" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer {ACCESS_TOKEN}" \
     -d '{
            "dataFormat": "json",
            "encoding": "UTF8",
            "jobName": "example_dataset_name-example-dataset_id at example_time",
            "notifications": [
            {
                "method": "email",
                "state": "failed_validation",
                "recipients": [
                "john@example.com"
                ]
            },
            {
                "method": "email",
                "state": "completed",
                "recipients": [
                "john@example.com"
                ]
            },
            ],
            "listDelimiter": ",",
            "source": "string",
            "rowLimit": 50000,
            "offset": 0,
            "columns": [
                "string"
            ],
            "keys": [
                "string"
            ],
            "keyRegex": "string",
            "exactMatch": {},
            "regexMatch": {},
            "dateFilterStart": "2022-12-07T22:29:07.446Z",
            "dateFilterEnd": "2022-12-07T22:29:07.446Z"
}'
```

#### Response

```json
{
  "datasetId": "6449b63563c1e069c6159415",
  "history": [
    {
      "timestamp": "2023-08-08 20:35:39",
      "jobState": "created",
      "message": "Created export job via API"
    },
    {
      "timestamp": "2023-08-08 20:35:39",
      "jobState": "queued",
      "message": "Job queued and ready for processing."
    }
  ],
  "imsOrgId": "0DFE76D95967D5B50A494010@AdobeOrg",
  "jobOptions": {
    "dataFormat": "tsv",
    "encoding": "utf8",
    "listDelimiter": ",",
    "rowLimit": 50000,
    "offset": 0,
    "dateFilterStart": "2023-05-01T22:29:07Z",
    "dateFilterEnd": "2023-08-08T14:35:38Z",
    "notifications": [
      {
        "method": "email",
        "state": "failed_validation",
        "recipients": [
          "john@example.com"
        ]
      },
      {
        "method": "email",
        "state": "completed",
        "recipients": [
          "john@example.com"
        ]
      },
    ]
  },
  "jobId": "16e38fbc-fc82-4fdf-88de-ec33e63489d5",
  "jobSize": -1,
  "name": "example_dataset_name-example-dataset_id at example_time",
  "setName": "example_name",
  "state": "queued",
  "totalLines": -1,
  "noeffectLines": null,
  "type": "export"
}
```

### Request example details

The example above requests the following:

* the `dataFormat` for the classification as `json`
* the `jobName` to be `example_dataset_name-example-dataset_id at example_time`
* the `notifications`to be delivered by `email` when the state is `failed_validation` and `completed`
* the `rowLimit` to be `50000`
* the `dateFilterStart` and `dateFilterEnd` are set to `2022-12-07T22:29:07.446Z` and `2022-12-07T22:29:07.446Z`

### Response example details

The example above returns the following:

* the dataset ID of `6449b63563c1e069c6159415`
* the `name` of `example_dataset_name-example-dataset_id at example_time`
* the classification `setName` of `example_name`
* the job `type` of `export`

### Request Parameters

The following table describes the POST export classification request parameters:

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| `dataset_id` | required | string | The dataset ID for creating an API export job |
| `dataFormat` | optional | string | The data format options. Includes `tsv`, `tab`, or `json`. |
| `encoding` | optional | string | The encoding for data. The default value is `UTF-8`. |
| `jobName` | optional | string | The name of the job |
| `listDelimiter` | optional | string | Specifies the data delimiter for the list. Default delimiter is `,` (comma) |
| `rowLimit` | optional | integer | The limit of included rows |
| `offset` | optional | integer |  |
| `columns` | optional | string | The included columns |
| `keys` | optional | string |  |
| `keyRegex` | optional | string | Key regular expression |
| `exactMatch` | optional |  |  |
| `regexMatch` | optional |  |  |
| `dateFilterStart` | optional | string | The first value in the date filter |
| `dateFilterEnd` | optional | string | The last value in the date filter |
| `dataUri` | optional | string | The data URI |
| `source` | optional | string | The data source. Default value is `"Direct API Upload"`. |
| `notifications` | optional | container | Contains the notification information. Includes the `method`, `state`, and `recipients` parameters. |
| `method` | optional | string | The method by which the notification is sent. This includes the enums `email` and `rabbit`. |
| `state` |  optional |string | The state of the notification. Includes the following enums: `created`, `queued`, `validated`, `failed_validation`, `processing`, `done_processing`, `failed_processing`, and `completed`. |
| `recipients` | optional | string | The recipients of the notification |

### Response Parameters

The following table describes the POST export classification response parameters:

| Name | Type | Description |
| --- | --- | --- |
| `setName` | string | The name of the classification dataset |
| `jobId` | string | The ID of the job |
| `jobSize` | integer | The size of the job file |
| `jobOptions` | container | The export options of the job |
| `name` | string | The name of the job |
| `totalLines` | integer | The total lines of the file |
| `datasetId` | string | The classification dataset ID of the job |
| `noeffectLines` | integer | The no effect lines of export job |
| `history` | container | The history of the job. Contains the `timestamp`, `jobState`, and `message` parameters. |
| `timestamp` | string | The time when the job state changed |
| `jobState` | string | The state of the job. Includes the enums: `created`, `queued`, `validated`, `failed_validation`, `processing`, `done_processing`, `failed_processing`, `completed`. |
| `message` | string | The details of the job history |
| `state` | string | The state of the job |
| `type` | string | The type of the job |
| `imsOrgId` | string | The ID associated with the analytics company of the user |

## GET export classification file

Use this endpoint to retrieve the contents of an export classification file. When using this endpoint, you must supply the export job ID returned with the POST export classification endpoint. For more information on classification data, see [Classification data files](https://experienceleague.adobe.com/docs/analytics/components/classifications/classifications-importer/c-saint-data-files.html).

`GET https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/job/export/file/{JOB_ID}`

### Request and Response Examples

Request and response examples
Click the **Request** tab in the following example to see a cURL request for this endpoint. Click the **Response** tab to see a successful JSON response for the request.

<CodeBlock slots="heading, code" repeat="2" languages="CURL,JSON"/>

#### Request

```sh
curl -X GET "https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/job/export/file/{JOB_ID}" \
     -H "x-api-key: {CLIENT_ID}" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer {ACCESS_TOKEN}"
```

#### Response

```tab
## SC   SiteCatalyst saint Import File  v:2.1
## SC   '## SC' indicates a SiteCatalyst pre-process header. Please do not remove these lines.
## SC   D:2023-07-31 18:00:11  A:xxxxxx:xx
 
Key File Name   File Option Directory
Key20230522-1   File Name-1 Option-1    Directory-1
Key20230522-2   File Name-2 Option-2    Directory-2
Key20230522-3   File Name-3 Option-3    Directory-3
Key20230522-4   File Name-4 Option-4    Directory-4
Key20230522-5   File Name-5 Option-5    Directory-5
Key20230522-6   File Name-6 Option-6    Directory-6
Key20230522-7   File Name-7 Option-7    Directory-7
Key20230522-8   File Name-8 Option-8    Directory-8
Key20230522-9   File Name-9 Option-9    Directory-9
```

### Request example details

The example above requests the export file of `{JOB_ID}`

### Response example details

The example above returns a table containing the exported data

### Request Parameters

The following table describes the GET export classification file request parameters:

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| `job_id` | required | string | Export Job ID |

### Response Parameters

No response parameters are returned. The response includes the classification data in the format specified with the export job request.

## GET classification template

Use this endpoint to retrieve a template showing how to structure exported data. For more information on how to structure your classification files, see [Classification data files](https://experienceleague.adobe.com/docs/analytics/components/classifications/classifications-importer/c-saint-data-files.html) and [Classification template](https://experienceleague.adobe.com/docs/analytics/components/classifications/classifications-importer/c-download-saint-data.html).

`GET  https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/datasets/template/{DATASET_ID}`

### Request and Response Examples

Request and response examples
Click the **Request** tab in the following example to see a cURL request for this endpoint. Click the **Response** tab to see a successful JSON response for the request.

<CodeBlock slots="heading, code" repeat="2" languages="CURL,JSON"/>

#### Request

```sh
curl -X GET "https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/datasets/template/{DATASET_ID}" \
     -H "x-api-key: {CLIENT_ID}" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer {ACCESS_TOKEN}"
```

#### Response

```tab
## SC   SiteCatalyst saint Import File  v:2.1
## SC   '## SC' indicates a SiteCatalyst pre-process header. Please do not remove these lines.
## SC   D:2023-07-31 23:13:42  A:xxxxxx:xx
 
Key Column A    Column B    Column C    Column D
```

### Request example details

The example above requests a template showing how to structure data

### Response example details

The example above returns a template showing how to structure data

### Request Parameters

The following table describes the GET classification template file request parameters.

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| `dataset_id` | required | string | Classification dataset ID |
| `format` | optional | string | Template format. The default is `tsv`. Includes the enums: `tsv`, `csv`. |

### Response Parameters

No response parameters are returned. The response includes a sample structure of how the data will be formatted while using the GET export classification file endpoint.

## GET all classification datasets

Use this endpoint to retrieve all datasets for a specified report suite ID. For more information on classification sets, see [Classification sets overview](https://experienceleague.adobe.com/docs/analytics/components/classifications/sets/overview.html).

`GET  https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/datasets/compatibilityMetrics/{RSID}`

### Request and Response Examples

Request and response examples
Click the **Request** tab in the following example to see a cURL request for this endpoint. Click the **Response** tab to see a successful JSON response for the request.

<CodeBlock slots="heading, code" repeat="2" languages="CURL,JSON"/>

#### Request

```sh
curl -X GET "https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/datasets/compatibilityMetrics/{RSID}" \
     -H "x-api-key: {CLIENT_ID}" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer {ACCESS_TOKEN}"
```

#### Response

```JSON
{
  "report_suite_id": "example_RSID",
  "metrics": [
    {
      "id": [
        "evar5"
      ],
      "datasets": [
        "646e65bfc2d00205dbc5f034"
      ]
    },
    {
      "id": [
        "evar11"
      ],
      "datasets": [
        "64c437bc6f71754e2e56e019",
        "64c437886f71754e2e56e011",
        "64c437396f71754e2e56e005",
        "64c437616f71754e2e56e009"
      ]
    },
  ]
}
```

### Request example details

The example above requests the classification datasets associated with the given `{RSID}`

### Response example details

The example above returns the following:

* the `report_suite_id` that is associated with the datsets is `example_RSID`
* the `metrics` associated with the `example_RSID`

### Request Parameters

The following table describes the GET all classification datasets request parameters.

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| `rsid` | required | string | Adobe Analytics report suite ID |

### Response Parameters

The following table describes the GET all classification datasets response parameters.

| Name | Type | Description |
| --- | --- | --- |
| `metrics` | container | Contains the `id`, and `datasets` parameters |
| `id` | string | ID |
| `datasets` | string | Datasets |
| `report_suite_id` | string | The report suite ID |

## GET a single classification

Use this endpoint to retrieve information for a specified dataset. For more information on classification sets, see [Classification sets overview](https://experienceleague.adobe.com/docs/analytics/components/classifications/sets/overview.html).

`GET https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/datasets/{DATASET_ID}`

### Request and Response Examples

Request and response examples
Click the **Request** tab in the following example to see a cURL request for this endpoint. Click the **Response** tab to see a successful JSON response for the request.

<CodeBlock slots="heading, code" repeat="2" languages="CURL,JSON"/>

#### Request

```sh
curl GET "https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/datasets/{DATASET_ID}"
     -H "x-api-key: {CLIENT_ID}" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer {ACCESS_TOKEN}"
```

#### Response

```JSON
{
  "name": "taxoappsrvtest evar7 classifications",
  "description": "taxoappsrvtest evar7 classifications",
  "default_list_delimiter": ",",
  "default_encoding": "UTF8",
  "columns": [
    {
      "column_id": "5849115441721086447",
      "name": "Column A",
      "display_name": "Column A",
      "type": "text",
      "tags": []
    },
    {
      "column_id": "495411758226162142",
      "name": "Column B",
      "display_name": "Column B",
      "type": "text",
      "tags": []
    },
  ],
  "subscriptions": [
    {
      "rsid": "taxoappsrvtest",
      "dimension": "variables/evar7",
      "unique": true,
      "editable": true
    }
  ],
  "notifications": [],
  "dataset_id": "64b1d77b235090539e282308",
  "ims_org_id": "0DFE76D95967D5B50A494010@AdobeOrg",
  "owner": {
    "name": "Unknown User",
    "email": "no-reply@adobe.com"
  },
  "last_modified_date": "2023-07-14T23:35:10Z"
}
```

### Request example details

The example above requests the data associated with the given `{DATASET_ID}`

### Response example details

The example above returns the following:

* the `name` of the dataset is `taxoappsrvtest evar7 classifications`
* the `columns` inside the dataset are `5849115441721086447` and `495411758226162142`
* the `name` of the columns are `"Column A"` and `"Column B"`
* the `subscriptions` associated with the dataset

### Request Parameters

The following table describes the request parameters for this endpoint:

| Name | Required | Type | Description |
| --- | --- | ---| --- |
| `dataset_id` | required | string | Classification Dataset ID |

### Response Parameters

The following table describes the response parameters for this endpoint:

| Parameter | Type | Description |
| --- | --- | --- |
| `owner` | container | Contact information that contains the `name` and `email` parameters |
| `name` | string | Name of person responsible for the classification definition and/or data |
| `email` | string | Email address for person responsible for the classification definition and/or data |
| `subscriptions` | container | A list of classification subscriptions. Subscriptions are not required at the time of creation but no data will be classified until at least one subscription exists. Contains the `rsid`, `dimension`, `unique`, and `editable` parameters. See [Classification settings](https://experienceleague.adobe.com/docs/analytics/components/classifications/sets/manage/settings.html) for more information. |
| `rsid` | string | The report suite ID |
| `dimension` | string | The dimension you would like to be classified. Should be prefixed with `variables/`, e.g., `variables/page`. |
| `unique` | boolean | Whether a forced update of unique_hash is used to avoid duplicate subscriptions |
| `editable` | boolean | Whether the subscription is editable by the current user based on report suite permissions |
| `default_encoding` | string | Default encoding for jobs. Defaults to `utf8`. Includes the enums `utf8` and `latin1`. |
| `columns` | container | A list of classification column definitions. Column definitions are not required at the time of creation but no data will be classified until at least one column definition exists. Contains the `column_id`, `name`, `display_name`, `type`, `classified_by`, and `tags` parameters. |
| `column_id` | string | A UUID that is generated when a column is created |
| `name` | string | The name for this given data column. It cannot be changed after column creation. It also can be any valid UTF-8 string. |
| `display_name` | string | The display name for the given data column. The value can be changed. It can be any valid UTF-8 string. |
| `type` | string | Defaults to `text`. This cannot be changed after column creation. Includes the following enums: `text`, `integer`, `float`, and `list`. |
| `classified_by` | string | An optional classification dataset ID that classifies this column's data |
| `tags` | string | Internal column tags used to store info (like div nums) to support migration |
| `dataset_id` | string | An auto-generated ID value created by the system on creation of the dataset, in the form of an ObjectId |
| `name` | string | A friendly display name for users to easily identify the classification definition |
| `description` | string | A long description for the purpose of this classification set |
| `last_modified_date` | string | The last modified date/time of the classification set |
| `last_modified_by` | string | The email address of the last person that modified the classification set |
| `ims_ord_id` | string | The ID associated with the analytics company of the user |
| `default_list_delimiter` | string | The default delimiter for list column types. Defaults to `,` (comma). If you have no list columns, this field does not apply. |
| `notifications` | container | Contains the notification information. Includes the `method`, `state`, and `recipients` parameters. |
| `method` | string | The method by which the notification is sent. This includes the enums `email` and `rabbit`. |
| `state` | string | The state of the notification. Includes the following enums: `created`, `queued`, `validated`, `failed_validation`, `processing`, `done_processing`, `failed_processing`, and `completed`. |
| `recipients` | string | The recipients of the notification |

## GET classification job information

Use this endpoint to retrieve job information for a specified job ID. For more information about classification jobs, see [Classification set jobs manager](https://experienceleague.adobe.com/docs/analytics/components/classifications/sets/job-manager.html)

`GET https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/job/{JOB_ID}`

### Request and Response Examples

Request and response examples
Click the **Request** tab in the following example to see a cURL request for this endpoint. Click the **Response** tab to see a successful JSON response for the request.

<CodeBlock slots="heading, code" repeat="2" languages="CURL,JSON"/>

#### Request

```sh
curl -X GET "https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/job/{JOB_ID}"
     -H "x-api-key: {CLIENT_ID}" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer {ACCESS_TOKEN}"
```

#### Response

```JSON
{
  "datasetId": "6449b63563c1e069c6159415",
  "history": [
    {
      "timestamp": "2023-08-07 22:06:56",
      "jobState": "created",
      "message": "Created export job via API"
    },
    {
      "timestamp": "2023-08-07 22:06:56",
      "jobState": "queued",
      "message": "Job queued and ready for processing."
    },
    {
      "timestamp": "2023-08-07 22:06:56",
      "jobState": "processing",
      "message": "Started processing"
    },
    {
      "timestamp": "2023-08-07 22:06:58",
      "jobState": "completed",
      "message": "Successfully exported 50000/50000 records."
    }
  ],
  "imsOrgId": "0DFE76D95967D5B50A494010@AdobeOrg",
  "jobOptions": {
    "dataFormat": "tsv",
    "encoding": "utf8",
    "listDelimiter": ",",
    "rowLimit": 50000,
    "offset": 0,
    "dateFilterStart": "2023-05-01T22:29:07Z",
    "dateFilterEnd": "2023-08-07T16:06:55Z",
    "notifications": [
      {
        "method": "email",
        "state": "failed_validation",
        "recipients": [
          "john@example.com"
        ]
      },
      {
        "method": "email",
        "state": "failed_processing",
        "recipients": [
          "john@example.com"
        ]
      },
      {
        "method": "email",
        "state": "completed",
        "recipients": [
          "john@example.com"
        ]
      }
    ]
  },
  "jobId": "7dafc070-afec-4a8d-8187-24f572f7d0f8",
  "jobSize": 26410225,
  "name": "example_dataset_name-example-dataset_id at example_time",
  "setName": "taxoappsrvtest evar1 classifications",
  "state": "completed",
  "totalLines": 50000,
  "noeffectLines": null,
  "type": "export"
}
```

### Request example details

The example above requests the job information associated with the given `{JOB_ID}`

### Response example details

The example above returns the following:

* the `datasetId` the job is associated with
* the `history` of the job
* the `jobOptions` of the job
* the `name` of the job is `example_dataset_name-example-dataset_id at example_time`
* the `state` of the job is `completed`

### Request Parameters

The following table describes the GET classification job information request parameters:

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| `job_id` | required | string | Classification Job ID |

### Response Parameters

The following table describes the GET classification job information response parameters:

| Name | Type | Description |
| --- | --- | --- |
| `setName` | string | The name of the classification dataset |
| `jobId` | string | The ID of the job |
| `jobSize` | integer | The size of the job file |
| `jobOptions` | container | The import or export options of the job |
| `name` | string | The name of the job |
| `totalLines` | integer | The total lines of the file |
| `datasetId` | string | The classification dataset ID of the job |
| `noeffectLines` | integer | The no effect lines of import job |
| `history` | container | The history of the job. Contains the `timestamp`, `jobState`, and `message` parameters. |
| `timestamp` | string | The time when the job state changed |
| `jobState` | string | The state of the job. Includes the enums: `created`, `queued`, `validated`, `failed_validation`, `processing`, `done_processing`, `failed_processing`, `completed`. |
| `message` | string | The details of the job history |
| `state` | string | The state of the job |
| `type` | string | The type of the job |
| `imsOrgId` | string | The IMS Org ID of the job |

## PUT classification dataset update

Use this endpoint to update a classification dataset.

`PUT https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/datasets/{DATASET_ID}`

### Request and Response Examples

Request and response examples
Click the **Request** tab in the following example to see a cURL request for this endpoint. Click the **Response** tab to see a successful JSON response for the request.

<CodeBlock slots="heading, code" repeat="2" languages="CURL,JSON"/>

#### Request

```sh
curl -X PUT "https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification/datasets/{DATASET_ID}"
     -H "x-api-key: {CLIENT_ID}" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer {ACCESS_TOKEN}"
```

#### Response

```JSON
{
  "datasetId": "6449b63563c1e069c6159415",
  "history": [
    {
      "timestamp": "2023-08-07 22:06:56",
      "jobState": "created",
      "message": "Created export job via API"
    },
    {
      "timestamp": "2023-08-07 22:06:56",
      "jobState": "queued",
      "message": "Job queued and ready for processing."
    },
    {
      "timestamp": "2023-08-07 22:06:56",
      "jobState": "processing",
      "message": "Started processing"
    },
    {
      "timestamp": "2023-08-07 22:06:58",
      "jobState": "completed",
      "message": "Successfully exported 50000/50000 records."
    }
  ],
  "imsOrgId": "0DFE76D95967D5B50A494010@AdobeOrg",
  "jobOptions": {
    "dataFormat": "tsv",
    "encoding": "utf8",
    "listDelimiter": ",",
    "rowLimit": 50000,
    "offset": 0,
    "dateFilterStart": "2023-05-01T22:29:07Z",
    "dateFilterEnd": "2023-08-07T16:06:55Z",
    "notifications": [
      {
        "method": "email",
        "state": "failed_validation",
        "recipients": [
          "john@example.com"
        ]
      },
      {
        "method": "email",
        "state": "failed_processing",
        "recipients": [
          "john@example.com"
        ]
      },
      {
        "method": "email",
        "state": "completed",
        "recipients": [
          "john@example.com"
        ]
      }
    ]
  },
  "jobId": "7dafc070-afec-4a8d-8187-24f572f7d0f8",
  "jobSize": 26410225,
  "name": "Direct API Export job for dataset taxoappsrvtest evar1 classifications - 6449b63563c1e069c6159415 at 1691446015",
  "setName": "taxoappsrvtest evar1 classifications",
  "state": "completed",
  "totalLines": 50000,
  "noeffectLines": null,
  "type": "export"
}
```

### Request example details

The example above requests to update the job information associated with the given `{JOB_ID}`

### Response example details

The example above returns the following:

* the `datasetId` the job is associated with
* the `history` of the job
* the `jobOptions` of the job
* the `name` of the job is `example_dataset_name-example-dataset_id at example_time`
* the `state` of the job is `completed`

### Request Parameters

The following table describes the request parameters for this endpoint:

| Name | Required | Type | Description |
| --- | --- | ---| --- |
| `dataset_id` | required | string | Classification Dataset ID |
| `owner` | optional | container | Contact information that contains the `name` and `email` parameters |
| `name` | optional | string | Name of person responsible for the classification definition and/or data |
| `email` | optional | string | Email address for person responsible for the classification definition and/or data |
| `subscriptions` | required | container | A list of classification subscriptions. Subscriptions are not required at the time of creation but no data will be classified until at least one subscription exists. Contains the `rsid`, `dimension`, and `unique` parameters. |
| `rsid` | required | string | The report suite ID |
| `dimension` | required | string | The dimension you would like to be classified. Should be prefixed with `variables/`, e.g., `variables/page`. |
| `unique` | required | boolean | Whether a forced update of unique_hash is used to avoid duplicate subscriptions |
| `default encoding` | optional | string | Default encoding for jobs. Defaults to `utf8`. Includes the enums: `utf8` and `latin1`. |
| `columns` | required | container | A list of classification column definitions. Column definitions are not required at the time of creation but no data will be classified until at least one column definition exists. Contains the `name`, `display_name`, `type`, `classified_by`, and `tags` parameters. |
| `name` | required | string | The name for this given data column. It cannot be changed after column creation. It also can be any valid UTF-8 string. |
| `display_name` | required | string | The display name for the given data column. The value can be changed. It can be any valid UTF-8 string. |
| `type` | optional | string | Defaults to `text`. This cannot be changed after column creation. Includes the following enums: `text`, `integer`, `float`, and `list`. |
| `classified_by` | optional | string | An optional classification dataset ID that classifies this column's data |
| `tags` | optional | string | Internal column tags. Used to store info (like div nums) to support migration. |
| `name` | optional | string | A friendly display name for users to easily identify the classification definition |
| `description` | optional | string | A long description for the purpose of this classification set |
| `default_list_delimiter` | optional | string | The default delimiter for list column types. Defaults to `,` (comma). If you have no list columns, this field does not apply. |
| `notifications` | optional | container | Contains the notification information. Includes the `method`, `state`, and `recipients` parameters. |
| `method` | optional | string | The method by which the notification is sent. This includes the enums `email` and `rabbit`. |
| `state` | optional | string | The state of the notification. Includes the following enums: `created`, `queued`, `validated`, `failed_validation`, `processing`, `done_processing`, `failed_processing`, and `completed`. |
| `recipients` | optional | string | The recipients of the notification |

### Response Parameters

The following table describes the response parameters for this endpoint:

| Name | Type | Description |
| --- | --- | --- |
| `owner` | container | Contact information that contains the `name` and `email` parameters |
| `name` | string | Name of person responsible for the classification definition and/or data |
| `email` | string | Email address for person responsible for the classification definition and/or data |
| `subscriptions` | container | A list of classification subscriptions. Subscriptions are not required at the time of creation but no data will be classified until at least one subscription exists. Contains the `rsid`, `dimension`, `unique`, and `editable` parameters. |
| `rsid` | string | The report suite ID |
| `dimension` | string | The dimension you would like to be classified. Should be prefixed with `variables/`, e.g., `variables/page`. |
| `unique` | boolean | Whether a forced update of unique_hash is used to avoid duplicate subscriptions |
| `editable` | boolean | Whether the subscription is editable by the current user based on report suite permissions |
| `default encoding` | string | Default encoding for jobs. Defaults to `utf8`. Includes the enums: `utf8` and `latin1`. |
| `columns` | container | A list of classification column definitions. Column definitions are not required at the time of creation but no data will be classified until at least one column definition exists. Contains the `column_id`, `name`, `display_name`, `type`, `classified_by`, and `tags` parameters. |
| `column_id` | string | A UUID that will be generated when a column is created |
| `name` | string | The name for this given data column. It cannot be changed after column creation. It also can be any valid UTF-8 string. |
| `display_name` | string | The display name for the given data column. The value can be changed. It can be any valid UTF-8 string. |
| `type` | string | Defaults to `text`. This cannot be changed after column creation. Includes the following enums: `text`, `integer`, `float`, and `list`. |
| `classified_by` | string | An optional classification dataset ID that classifies this column's data |
| `tags` | string | Internal column tags. Used to store info (like div nums) to support migration |
| `dataset_id` | string | An auto-generated ID value created by the system on creation of the dataset, in the form of an ObjectId |
| `name` | string | A friendly display name for users to easily identify the classification definition |
| `description` | string | A long description for the purpose of this classification set |
| `last_modified_date` | string | Last modified date/time of the classification set |
| `last_modified_by` | string | Email address of the last person that modified the classification set |
| `ims_org_id` | string | The ID associated with the analytics company of the user |
| `default_list_delimiter` | string | The default delimiter for list column types. Defaults to `,` (comma). If you have no list columns, this field does not apply. |
| `notifications` | container | Contains the notification information. Includes the `method`, `state`, and `recipients` parameters. |
| `method` | string | The method by which the notification is sent. This includes the enums `email` and `rabbit`. |
| `state` | string | The state of the notification. Includes the following enums: `created`, `queued`, `validated`, `failed_validation`, `processing`, `done_processing`, `failed_processing`, and `completed`. |
| `recipients` | string | The recipients of the notification |

## DELETE classification

Use this endpoint to delete a specified classification. For information, see [Delete classification data](https://experienceleague.adobe.com/docs/analytics/components/classifications/classifications-importer/t-delete-classification-data.html)

`DELETE https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification//datasets/{DATASET_ID}`

### Request and Response Examples

Request and response examples
Click the **Request** tab in the following example to see a cURL request for this endpoint. Click the **Response** tab to see a successful JSON response for the request.

<CodeBlock slots="heading, code" repeat="2" languages="CURL,JSON"/>

#### Request

```sh
curl -X PUT "https://analytics.adobe.io/api/{GLOBAL_COMPANY_ID}/classification//datasets/{DATASET_ID}"
     -H "x-api-key: {CLIENT_ID}" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer {ACCESS_TOKEN}"
```

#### Response

```JSON
{
  "success": true,
  "message": "The dataset 123456 has been deleted"
}
```

### Request example details

The example above requests to delete the given `{DATASET_ID}`

### Response example details

The example above returns the following:

* the `success` of the job is `true`
* the `message` given is `The dataset 123456 has been deleted`

### Request Parameters

The following table describes the DELETE classification request parameters:

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| `dataset_id` | required | string | Classification Dataset ID |

### Response Parameters

No response parameters are returned. A `200` signals a successful DELETE.

## API status codes

For a description of API status codes and tips for troubleshooting, see the [Platform FAQ and troubleshooting guide](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html#api-status-codes).