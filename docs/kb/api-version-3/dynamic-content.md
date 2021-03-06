# Querying Dynamic Content


## Supported Operations<a name="62781-supported-operations" id="supported-operations"></a>

| **Operation** | **URL Format**                             | **Required Parameters** | **Description**  |
| ------------- | ------------------------------------------ | ----------------------- | -----------------|
| `query` | `/api/dynamicContent/version/3/do/query?...` | `user_key, api_key` | Returns the dynamic content matching the specified criteria parameters. See the [Using Dynamic Content](#using-dynamic-content) section for a complete description of the dynamic content [XML Response Format](#xml-response-format). Also see [Dynamic Content](../object-field-references.md#dynamic-content) in [Object Field References](../object-field-references.md) |

<a name="62781-supported-search-criteria" id="supported-search-criteria"></a>

## Supported Search Criteria

Search criteria may be used together in any combination and/or order unless otherwise specified. Unless `output=mobile` is specified, 200 dynamic content will be returned with each query request. This limit is not enforced for responses formatted for mobile devices.


| **Parameter** | **Datatype**                               | **Options**             | **Description**  |
| ------------- | ------------------------------------------ | ----------------------- | -----------------|
| `created_after` | `string` | `today, yesterday, last_7_days, this_month, last_month, <custom_time>` | Searches for dynamic content that were created after the specified time. If a `<custom_time>` is used, ensure that the specified date is formatted using [GNU Date Input Syntax](http://www.gnu.org/software/tar/manual/html_node/Date-input-formats.html) |
| `created_before` | `string` | `today, yesterday, last_7_days, this_month, last_month, <custom_time>` | Searches for dynamic content that were created before the specified time. If a `<custom_time>` is used, ensure that the specified date is formatted using [GNU Date Input Syntax](http://www.gnu.org/software/tar/manual/html_node/Date-input-formats.html) |
| `id_greater_than` | `integer` | `<any_positive_integer>` | Searches for dynamic content with IDs greater than the specified integer |
| `id_less_than` | `integer` | `<any_positive_integer>` | Searches for dynamic content with IDs less than the specified integer |
| `updated_before` | `string` | `today, yesterday, last_7_days, this_month, last_month, <custom_time>` | Searches for dynamic content that were updated before the specified time. If a `<custom_time>` is used, ensure that the specified date is formatted using [GNU Date Input Syntax](http://www.gnu.org/software/tar/manual/html_node/Date-input-formats.html) |
| `updated_after` | `string` | `today, yesterday, last_7_days, this_month, last_month, <custom_time>` | Searches for dynamic content that were updated after the specified time. If a `<custom_time>` is used, ensure that the specified date is formatted using [GNU Date Input Syntax](http://www.gnu.org/software/tar/manual/html_node/Date-input-formats.html) |


## Manipulating the Result Set

Since `query` result sets are limited to 200 results each, the results returned may not include all the dynamic content matched by the query. To retrieve the remaining results, the following criteria can be used to navigate through the result set.

| **Parameter** | **Datatype**                               | **Options**             | **Description**  |
| ------------- | ------------------------------------------ | ----------------------- | -----------------|
| `limit` | `integer` | `<any_positive_integer>` | Specifies the number of results to be returned. _Default value:_ `200`. **_Note:_** This number cannot be larger than 200. If a number larger than 200 is specified, 200 will be used |
| `offset` | `integer` | `<any_positive_integer>` | Specifies the first matching dynamic content (according to the specified sorting order) to be returned in the `query` response. The first `offset` matching dynamic content will be omitted from the response. _Default value:_ `0`. **_Example:_** Specifying `offset=10` will return the results starting with the 11th dynamic content matched by the provided criteria |
| `sort_by` | `string` | `created_at, id` | Specifies the field that should be used to sort the results of the query. See [Supported Sorting Options](#supported-sorting-options) for more details. |
| `sort_order` | `string` | `ascending, descending` | Specifies the ordering to be used when sorting the results of the query. The default value varies based on the value of the `sort_by` parameter. See [Supporting Sorting Options](#supported-sorting-options) for more details |

## Supported Sorting Options

The ordering of the results returned by a `query` request can be changed by specifying `sort_by` and `sort_order` parameters. Any of the following values are valid when specifying the `sort_by` parameter. For a complete list of fields involved in dynamic content queries, see Dynamic Content in Object Field References.

| **Value** | **Default Sort Order** | **Description** |
| --------- | ---------------------- | --------------- |
| `created_at` | `descending` | Specifies that the query results should be sorted by the dynamic content's `created_at` timestamps |
| `id` | `ascending` | Specifies that the query results should be sorted by the dynamic content's `id` fields |
| `updated_at` | `descending` | Specifies that the query results should be sorted by the dynamic content's `updated_at` timestamps |


## XML Response Format

```
<rsp stat="ok" version="1.0">
    <result>
        <total_results>...</total_results>
        <dynamicContent>...</dynamicContent>
        <dynamicContent>...</dynamicContent>
        <dynamicContent>...</dynamicContent>
    </result>
</rsp>
```

| **Tag** | **Description** |
| ------- | --------------- |
| `<result>` | Contains the resulting dynamic content for the specified query |
| `<total_results>` | Contains the number of dynamic content selected by this query. If this value is higher than 200, then several query requests may be necessary to retrieve all of the matched dynamic content |
| `<dynamicContent>` | The data for an individual dynamic content. See [Using Dynamic Content](#using-dynamic-content) for a complete description of the dynamic content [XML Response Format](#xml-response-format). Also see [Dynamic Content](../object-field-references.md#dynamic-content) in [Object Field References](../object-field-references.md) |


# Using Dynamic Content


## Supported Operations<a name="62780-supported-operations" id="supported-operations"></a>

For a complete list of fields involved in user operations, see the [Dynamic Content](../object-field-references.md#dynamic-content) section of [Object Field References](../object-field-references.md).

| **Operation** | **URL Format**                             | **Required Parameters** | **Description**  |
| ------------- | ------------------------------------------ | ----------------------- | -----------------|
| `read` | `/api/dynamicContent/version/3/do/read/id/`**_`<id>`_**`?...` | `user_key, api_key, id` | Returns the data for the dynamic content specified by `<id>`. `<id>` is the Pardot ID of the target dynamic content. |


## XML Response Format

```
<rsp stat="ok" version="1.0">
  <dynamicContent>
    <id>17</id>
    <name>Piece Of Dynamic Content</name>
    <embedCode>
         <script type="text/javascript" src="http://www2.pardot.com/gie9a/1/17/dc.js"></script>
    </embedCode>
    <embedUrl>http://www2.pardot.com/gie9a/1/17/dc.js</embedUrl>
    <baseContent><p class="title">This is Dynamic Content</p></baseContent>
    <basedOn>Custom Field: CRM System</basedOn>
    <variation>
        <comparison>is salesforce.com</comparison>
        <content>
            <h1 class="title"><img src="http://www2.pardot.com/l/1/2012-05-23/6tfgex/1/140707/sfdc_nirvana.png" width="498" height="79" alt="sfdc-nirvana.png" title="sfdc-nirvana.png" /></h1>
        </content>
    </variation>
    <variation>
        <comparison>is SugarCRM</comparison>
        <content>
            <p><img src="http://www2.pardot.com/l/1/2012-05-23/t6fjkg/1/140717/sugarcrm_nirvana.png" width="560" height="73" alt="sugarcrm-nirvana.png" title="sugarcrm-nirvana.png" /></p>
        </content>
    </variation>
    <created_at>2012-04-13 10:19:54</created_at>
    <updated_at>2012-05-23 02:55:34</updated_at>
  </dynamicContent>
</rsp>
```

| **Tag** | **Description** |
| ------- | --------------- |
| `<form>` | Parent tag. Contains data fields for target form. For complete field listing, see [Form](../object-field-references.md#form) in [Object Field References](../object-field-references.md). |
| `<embedCode>` | Contains `<script>` of the dynamic content which you can use to embed the dynamic content on your webpage. |
| `<variation>` | Contains `<comparison>` and `<content>` of the variation for the dynamic content. |
| `<content>` | Contains the content that is shown for the specific variation. |
