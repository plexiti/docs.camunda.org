---

title: 'Migrate the Server'
category: 'Migrate from camunda bpm 7.0 to 7.1'

---

## JBoss AS 7.1.3 to 7.2.0

### Upgrade the JBoss application server

camunda bpm 7.1 ships with a new version of the JBoss AS 7, namely 7.2.0.
The preferred upgrade steps are:

* copy all your custom modules / subsystems to the new jboss server directory
* re-add all your custom modifications from the existing `$CAMUNDA_HOME/standalone/configuration/standalone.xml` to the new one located in the new jboss server directory.
* copy all deployed datasources to the new jboss server directory
* undeploy all process applications and copy them to the new jboss server directory for redeployment
