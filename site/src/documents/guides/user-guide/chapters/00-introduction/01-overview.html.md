---

title: 'Overview'
category: 'Introduction'

---


Welcome to the camunda BPM user guide! camunda BPM is a Java-based framework for process automation. This document contains information about the features provided by camunda BPM platform.

camunda BPM is built around the process engine component. The following illustration shows the most important components of camunda BPM along with typical user roles.

<center>
  <img class="img-responsive" src="ref:asset:/assets/img/architecture-overview.png"/>
</center>

*Process Engine & Infrastructure*

*   [Process Engine](ref:#process-engine) The process engine is a java library responsible for executing BPMN 2.0 processes and workflows. It has a lightweight POJO core and uses a relational database for persistence. ORM mapping is provided by the mybatis mapping framework.
*   [Spring Framework Integration](ref:#spring-framework-integration)
*   [CDI / Java EE Integration](ref:#cdi-and-java-ee-integration)
*   [Runtime Container Integration](ref:#runtime-container-integration) (Integration with application server infrastructure.)

*Web Applications*

*   [REST API](ref:/api-references/rest/) The REST API allows using the process engine from a remote application or a Java Script application. (Note: The documentation of the REST API is factored out into an own document.)
*   [camunda Tasklist](ref:#tasklist) A web application for human workflow management and user tasks. The task list allows process participants to inspect their workflow tasks and navigate to task forms in order to work on the tasks and provide data input.
*   [camunda Cockpit](ref:#cockpit) A web application for process monitoring and operations. camunda Cockpit allows to search for process instanced, inspect their state and repair broken instances.
*   [camunda Cycle](ref:#cycle) A web application for synchronizing BPMN 2.0 process models between different modeling tools and modelers.

*Additional Tools*

*   [camunda Modeler](http://camunda.org/features/modeler.html): Eclipse plugin for process modeling.
*   [camunda-bpmn.js](https://github.com/camunda/camunda-bpmn.js): JavaScript framework for parsing, rendering and executing BPMN 2.0 from XML source.
