---

title: 'JSF Task Forms'
category: 'User Interface'

---


## Adding JSF Forms to your process application

If you add JSF forms as decribed below you can easily use them as [external task forms](ref:/guides/user-guide/#tasklist-task-forms-external-task-forms). 

Working examples can be found in [our examples](ref:/real-life/examples/).

The BPMN process used for this example is shown in the following picture:

<center>
  <img src="ref:asset:/assets/img/real-life/jsf-task-forms/task-form-process.png" class="img-responsive"/>
</center>

In this process model we added so called form keys to

 * the Start Event "invoice received". This is the form the user has to complete for starting a new process instance. 
 * the User Tasks. These are the forms the user has to complete when completing user tasks that are assigned to him.

This is how it looks like in the BPMN 2.0 XML: 

```xml
<startEvent id="start" camunda:formKey="sample-start-form" name="invoice received" />
<userTask id="file-invoice" camunda:assignee="kermit" camunda:formKey="sample-task-form-2" name="File Invoice" />
<userTask id="categorize-invoice" camunda:assignee="kermit" camunda:formKey="sample-task-form-1" name="Categorize Invoice" />
<endEvent id="end" name="invoice categorized" />
...
```

## Creating Simple User Task Form

Create a normal JSF page in `src/main/webapp/WEB_INF` representing a form used for User Tasks. Shown below is a very simple task form:

```xml
<!DOCTYPE HTML>
<html lang="en" xmlns="http://www.w3.org/1999/xhtml"
  xmlns:ui="http://java.sun.com/jsf/facelets"
  xmlns:h="http://java.sun.com/jsf/html"
  xmlns:f="http://java.sun.com/jsf/core">
<h:head>
  <f:metadata>
    <f:event type="preRenderView" listener="#{camunda.taskForm.startTaskForm()}" />
  </f:metadata>
  <title>Task Form: #{task.name}</title>
</h:head>
<h:body>
  <h1>#{task.name}</h1>
  <h:form id="someForm">
    <p>Here you would see the actual form to work on the task in some design normally either matching you task list or your business application (or both in the best case).</p>
    <h:commandButton id="submit_button" value="task completed" action="#{camunda.taskForm.completeTask()}" />
  </h:form>
</h:body>
</ui:composition>
```

Note that you need `camunda-engine-cdi` in order to have the `camunda.taskForm` bean available.


## How does this work?

If the user clicks on "work on task" in the tasklist, he will follow a link to this form, including the taskId and the callback URL (the URL to access the central tasklist) as GET-Parameters. Accessing this form will trigger the special CDI bean `camunda.taskForm` which

 *   starts a conversation,
 *   remembers the callback URL 
 *   starts the User Task in the process engine, meaning the bean sets the start date and assingns the task to the CDI business process scope (see [CDI Integration](ref:/guides/user-guide/#cdi-and-java-ee-integration) for details). 

Therefor you just need this code block:

```xml
<f:metadata>
  <f:event type="preRenderView" listener="#{camunda.taskForm.startTaskForm()}" />
</f:metadata>
```

Submit the form by calling the `camunda.taskForm` bean again which

*   completes the task in the process engine, causing the current token to advance in the process,
*   ends the conversation,
*   triggers a redirect to the callback URL of the tasklist.

    ```xml
    <h:commandButton id="submit_button" value="task completed" action="#{camunda.taskForm.completeTask()}" />
    ```

Note that the command button doesn't have to be contained on the same form, you might have a whole wizard containing multiple forms in a row before having the completeTask button. This will work because of the conversation running in the background.


## Access process variables

In the forms you can access your own CDI beans as usual and also access the camunda CDI beans. This makes it easy to access process variables, e.g. via the processVariables CDI bean:

```xml
<h:form id="someForm">
  <p>Here you would see the actual form to work on the task in some design normally either matching you task list or your business application (or both in the best case).</p>
  <table>
    <tr>
      <td>Process variable <strong>x</strong> (given in in the start form):</td><td><h:outputText value="#{processVariables['x']}" /></td>
    </tr>
    <tr>
      <td>Process variable <strong>y</strong> (added in this task form):</td><td><h:inputText value="#{processVariables['y']}" /></td>
    </tr>
    <tr>
      <td></td><td><h:commandButton id="submit_button" value="task completed" action="#{camunda.taskForm.completeTask()}" /></td>
    </tr>
  </table>
</h:form>
```

This is rendered to a simple form

<center>
  <img src="ref:asset:/assets/img/real-life/jsf-task-forms/variablesTaskFormExample.png" class="img-responsive"/>
</center>

The same mechanism can be used to start a new process instance.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ui:composition xmlns="http://www.w3.org/1999/xhtml"
  xmlns:ui="http://java.sun.com/jsf/facelets"
  xmlns:f="http://java.sun.com/jsf/core"
  xmlns:h="http://java.sun.com/jsf/html">
 
<h:head>
  <f:metadata>
    <f:event type="preRenderView" listener="#{camunda.taskForm.startProcessInstanceByKeyForm()}" />
  </f:metadata>
  <title>Start Process: #{camunda.taskForm.processDefinition.name}</title>
</h:head>
<h:body>
  <h1>#{camunda.taskForm.processDefinition.name}</h1>
  <p>Start a new process instance in version: #{camunda.taskForm.processDefinition.version}</p>
  <h:form id="someForm">
    <p>Here you see the actual form to start a new process instance, normally this would be in some design  either matching you task list or your business application (or both in the best case).</p>
    <table>
      <tr>
        <td>Process variable <strong>x</strong>:</td><td><h:inputText value="#{processVariables['x']}" /></td>
      </tr>
      <tr>
        <td></td><td><h:commandButton id="submit_button" value="start process instance" action="#{camunda.taskForm.completeProcessInstanceForm()}" /></td>
      </tr>
    </table>
  </h:form>
</h:body>
</ui:composition>
```

<center>
  <img src="ref:asset:/assets/img/real-life/jsf-task-forms/startFormExample.png" class="img-responsive"/>
</center>

If the user clicks on "Start Process" in the tasklist and chooses the process your start form is assigned to, he will follow a link to this form, including the processDefinitionKey and the callback URL (the URL to access the central tasklist) as GET-Parameters. Accessing this form will trigger the special CDI bean "camunda.taskForm" which

*   starts a conversation,
*   remembers the callback URL to the centralized tasklist.

You need this code block in your JSF page:

```xml
<f:metadata>
  <f:event type="preRenderView" listener="#{camunda.taskForm.startProcessInstanceByIdForm()}" />
</f:metadata>
```

Submiting the start form now

 * starts the process instance in the process engine,
 * ends the conversation,
 * triggers a redirect to the callback URL of the tasklist.

```xml
<h:commandButton id="submit_button" value="start process instance" action="#{camunda.taskForm.completeProcessInstanceForm()}" />
```

Note that the command button doesn't have to be contained on the same form, you might have a whole wizard containing multiple forms in a row before having the completeProcessInstanceForm button. This will work because of the conversation running in the background.


## Styling your task forms

We use [Twitter Bootstrap](http://getbootstrap.com/) in our tasklist - so best add this to your Process Application as well and you can easily polish your UI: 

<center>
  <img src="ref:asset:/assets/img/real-life/jsf-task-forms/tasklist-forms-layouted-start.png" class="img-responsive"/>
</center>

Just include the needed CSS and Javascript libraries in the header part of your forms. If you have several forms, it may be helpful to create a template you can refer to from your forms to avoid redundancies. 

```xml
<h:head>
  <title>your title</title>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />

  <!-- CSS Stylesheets -->
  <h:outputStylesheet name="css/bootstrap.css" />
  <h:outputStylesheet name="css/responsive.css" />

  <!-- Javascript Libraries -->
  <h:outputScript name="js/jquery.js"/>
  <h:outputScript name="js/bootstrap.js"/>
</h:head>
```