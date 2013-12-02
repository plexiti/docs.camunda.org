---

title: 'Get Job Definitions Count'
category: 'Job Definition'

keywords: 'get query list'

---


Query for the number of job definitions that fulfill given parameters.
Takes the same parameters as the [get job definitions](ref:#job-definition-get-job-definitions) method.


Method
------

GET <code>/job-definition/count</code>


Parameters
----------  
  
#### Query Parameters

<table class="table table-striped">
  <tr>
    <th>Name</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>jobDefinitionId</td>
    <td>Filter by job definition id.</td>
  </tr>
  <tr>
    <td>activityIdIn</td>
    <td>Only include job definitions which belongs to one of the passed and comma-separated activity ids.</td>
  </tr>
  <tr>
    <td>processDefinitionId</td>
    <td>Only include job definitions which exist for the given process definition id.</td>
  </tr>
  <tr>
    <td>processDefinitionKey</td>
    <td>Only include job definitions which exist for the given process definition key.</td>
  </tr>
  <tr>
    <td>jobType</td>
    <td>Only include job definitions which exist for the given job type.</td>
  </tr>
  <tr>
    <td>jobConfiguration</td>
    <td>Only include job definitions which exist for the given job configuration.</td>
  </tr>
  <tr>
    <td>active</td>
    <td>Only include active job definitions.</td>
  </tr>
  <tr>
    <td>suspended</td>
    <td>Only include suspended job definitions.</td>
  </tr>
  <tr>
    <td>sortBy</td>
    <td>Sort the results lexicographically by a given criterion. Valid values are
    <code>jobDefinitionId</code>, <code>activityId</code>, <code>processDefinitionId</code>, <code>processDefinitionKey</code>, <code>jobType</code> and <code>jobConfiguration</code>.
    Must be used in conjunction with the <code>sortOrder</code> parameter.</td>
  </tr>  
  <tr>
    <td>sortOrder</td>
    <td>Sort the results in a given order. Values may be <code>asc</code> for ascending order or <code>desc</code> for descending order.
    Must be used in conjunction with the <code>sortBy</code> parameter.</td>
  </tr>
</table>

Result
------

A json object that contains the count as the only property.

<table class="table table-striped">
  <tr>
    <th>Name</th>
    <th>Value</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>count</td>
    <td>Number</td>
    <td>The number of matching executions.</td>
  </tr>
</table>

Response codes
--------------  

<table class="table table-striped">
  <tr>
    <th>Code</th>
    <th>Media type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>200</td>
    <td>application/json</td>
    <td>Request successful.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>application/json</td>
    <td>Returned if some of the query parameters are invalid, for example if a <code>sortOrder</code> parameter is supplied, but no <code>sortBy</code>. See the <a href="ref:#overview-introduction">Introduction</a> for the error response format.</td>
  </tr>
</table>


Example
-------

#### Request

<!-- TODO: Insert a 'real' example -->
GET <code>/job-definition/count?activityIdIn=ServiceTask1,ServiceTask2</code>
  
#### Response

    {"count": 2}
