---

title: 'Error Events'
category: 'Events'

keywords: 'error start end boundary event definition'

---


Error events are events which are triggered by a defined error.

__Important note:__ a BPMN error is NOT the same as a Java exception. In fact, the two have nothing in common. BPMN error events are a way of modeling business exceptions. Java exceptions are handled in their own specific way.

<div class="alert alert-warning">
   <strong>Heads up!</strong> You might want to check out the basics of <a href="https://app.camunda.com/confluence/display/foxUserGuide/Error+Handling">error handling</a> first.
</div>

<div id="errorEvent" style="position:relative" data-bpmn-src="implement/event-error"></div>

An error event definition references an error element. The following is an example of an error end event, referencing an error declaration:

<div class="app-source" app-source-no-tabs="error1"></div>
<script type="text/xml" id="error1">
<definitions>
<error id="myError" errorCode="ERROR-OCCURED" name="ERROR-OCCURED"/>
...
<process>
  ...
  <endEvent id="myErrorEndEvent">
    <errorEventDefinition errorRef="myError" />
  </endEvent>
</process>
</definitions>
</script>

Another possibility to define an error is the setting of an exception type as error code. The following is an example of an exception type as error code:

<div class="app-source" app-source-no-tabs="exception1"></div>
<script type="text/xml" id="exception1">
<definitions>
<error id="myException" errorCode="com.company.MyBusinessException" name="myBusinessException"/>
...
<process>
  ...
  <endEvent id="myErrorEndEvent">
    <errorEventDefinition errorRef="myException" />
  </endEvent>
</process>
</definitions>
</script>

The exception type should only used for business exceptions and not for technical exceptions in the process.

An error event handler references the same error element to declare that it catches the error. 


## Error Start Event

An error start event can only be used to trigger an Event Sub-Process - it __cannot__ be used for starting a process instance. The error start event is always interrupting.

<div id="errorEvent2" style="position:relative" data-bpmn-src="implement/event-subprocess-alternative1"></div>


## Error End Event

When process execution arrives in an error end event, the current path of execution is ended and an error is thrown. This error can caught by a matching intermediate boundary error event. In case no matching boundary error event is found, the execution semantics default to the none end event semantics.


## Error Boundary Event

An intermediate catching error on the boundary of an activity, or boundary error event for short, catches errors that are thrown within the scope of the activity on which it is defined.

Defining a boundary error event makes most sense on an embedded subprocess, or a call activity, as a subprocess creates a scope for all activities inside the subprocess. Errors are thrown by error end events. Such an error will propagate its parent scopes upwards until a scope is found on which a boundary error event is defined that matches the error event definition.

When an error event is caught, the activity on which the boundary event is defined is destroyed, also destroying all current executions within (e.g. concurrent activities, nested subprocesses, etc.). Process execution continues following the outgoing sequence flow of the boundary event.

<div id="errorEvent3" style="position:relative" data-bpmn-src="implement/event-subprocess-alternative2"></div>

A boundary error event is defined as a typical boundary event. As with the other error events, the errorRef references an error defined outside the process element:

<div class="app-source" app-source-no-tabs="error2"></div>
<script type="text/xml" id="error2">
<definitions>
<error id="myError" errorCode="ERROR-OCCURED" name="name of error"/>
...
<process>
  ...
  <subProcess id="mySubProcess">
    ...
  </subProcess>
  <boundaryEvent id="catchError" attachedToRef="mySubProcess">
    <errorEventDefinition errorRef="myError"/>
  </boundaryEvent>
</process>
</definitions>
</script>

The errorCode is used to match the errors that are caught:

* If errorRef is omitted, the boundary error event will catch any error event, regardless of the errorCode of the error.
* In case an errorRef is provided and it references an existing error, the boundary event will only catch errors with the same error code.


## Additional Resources

* [Error Events in the BPMN Tutorial](http://camunda.org/design/reference.html#!/events/error)
* [Error Handling](https://app.camunda.com/confluence/display/foxUserGuide/Error+Handling)