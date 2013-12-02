---

title: 'Get Job Definitions'
category: 'Job Definition'

keywords: 'get query list'

---


Query for job definitions that fulfill given parameters. 
The size of the result set can be retrieved by using the [get job definitions count](ref:#job-definition-get-job-definitions-count) method.


Method
------

GET <code>/job-definition</code>


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

A json array of job definition objects.
Each job definition object has the following properties:

<table class="table table-striped">
  <tr>
    <th>Name</th>
    <th>Value</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>id</td>
    <td>String</td>
    <td>The id of the job definition.</td>
  </tr>
  <tr>
    <td>processDefinitionId</td>
    <td>String</td>
    <td>The id of the process definition this job definition is associated with.</td>
  </tr>
  <tr>
    <td>processDefinitionKey</td>
    <td>String</td>
    <td>The key of the process definition this job definition is associated with.</td>
  </tr>
  <tr>
    <td>activityId</td>
    <td>String</td>
    <td>The id of the activity this job definition is associated with.</td>
  </tr>  
  <tr>
    <td>jobType</td>
    <td>String</td>
    <td>The type of the job which are running for this job definition. For example: asynchronous continuation, timer etc.</td>
  </tr>
  <tr>
    <td>jobConfiguration</td>
    <td>String</td>
    <td>The configuration of a job definition provides details about the jobs wicht will be created. For timer jobs it is for example the timer configuration.</td>
  </tr>
  <tr>
    <td>suspended</td>
    <td>Boolean</td>
    <td>Indicates whether this job definition is suspended.</td>
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
GET <code>/job-definition?activityIdIn=ServiceTask1,ServiceTask2</code>
  
#### Response

    [
      {
        "id": "aJobDefId",
        "processDefinitionId": "aProcDefId",
        "processDefinitionKey": "aProcDefKey",
        "activityId": "ServiceTask1",
        "jobType": "asynchronous-continuation",
        "jobConfiguration": "",
        "suspended": false
      },
      {
        "id": "aJobDefId",
        "processDefinitionId": "aProcDefId",
        "processDefinitionKey": "aProcDefKey",
        "activityId": "ServiceTask2",
        "jobType": "asynchronous-continuation",
        "jobConfiguration": "",
        "suspended": true
      }
    ]
