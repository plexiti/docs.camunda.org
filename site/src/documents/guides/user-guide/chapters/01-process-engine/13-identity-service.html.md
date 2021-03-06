---

title: 'Identity Service'
category: 'Process Engine'

---

The identity service is an API abstraction over various User / Group repositories. The basic entities are

* User: a user identified by a unique Id
* Group: a group identified by a unique Id
* Membership: the relationship between users and groups

Example:

    User demoUser = processEngine.getIdentityService()
      .createUserQuery()
      .userId("demo")
      .singleResult();

camunda BPM distinguishes between read-only and writable user repositories. A read-only user repository provides read-only access to the underlying user / group database. A writable user repository allows write access to the user database which includes creating, updating and deleting users and groups.

In order to provide a custom identity provider implementation, the following interfaces can be implemented:

* [org.camunda.bpm.engine.impl.identity.ReadOnlyIdentityProvider](ref:/api-references/javadoc/?org/camunda/bpm/engine/impl/identity/ReadOnlyIdentityProvider.html)
* [org.camunda.bpm.engine.impl.identity.WritableIdentityProvider](ref:/api-references/javadoc/?org/camunda/bpm/engine/impl/identity/WritableIdentityProvider.html)

## The Database Identity Service

The database identity service uses the process engine database for managing users and groups. This is the default identity service implementation used if no alternative identity service implementation is provided.

The Database Identity Service implements both `ReadOnlyIdentityProvider` and `WritableIdentityProvider` providing full CRUD functionality in Users, Groups and Memberships.

## The LDAP Identity Service

The LDAP identity service provides read-only access to an LDAP-based user / group repository. The identity service provider is implemented as a [Process Engine Plugin](ref:#process-engine-process-engine-plugins) and can be added to the process engine configuration. In that case it replaces the default Database Identity Service.

In order to use the LDAP identity service, the `camunda-identity-ldap.jar` library has to be added to the classloader of the process engine.

    <dependency>
      <groupId>org.camunda.bpm.identity</groupId>
      <artifactId>camunda-identity-ldap</artifactId>
      <version>${camunda.version}</version>
    </dependency>


The following is an example of how to configure the LDAP Identity Provider Plugin using Spring XML:

    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans   http://www.springframework.org/schema/beans/spring-beans.xsd">
      <bean id="processEngineConfiguration" class="org.camunda.bpm.engine.impl.cfg.StandaloneInMemProcessEngineConfiguration">
        ...
        <property name="processEnginePlugins">
          <list>
            <ref bean="ldapIdentityProviderPlugin" />
          </list>
        </property>
      </bean>
      <bean id="ldapIdentityProviderPlugin" class="org.camunda.bpm.identity.impl.ldap.plugin.LdapIdentityProviderPlugin">
        <property name="serverUrl" value="ldap://localhost:3433/" />
        <property name="managerDn" value="uid=daniel,ou=office-berlin,o=camunda,c=org" />
        <property name="managerPassword" value="daniel" />
        <property name="baseDn" value="o=camunda,c=org" />

        <property name="userSearchBase" value="" />
        <property name="userSearchFilter" value="(objectclass=person)" />
        <property name="userIdAttribute" value="uid" />
        <property name="userFirstnameAttribute" value="cn" />
        <property name="userLastnameAttribute" value="sn" />
        <property name="userEmailAttribute" value="mail" />
        <property name="userPasswordAttribute" value="userpassword" />

        <property name="groupSearchBase" value="" />
        <property name="groupSearchFilter" value="(objectclass=groupOfNames)" />
        <property name="groupIdAttribute" value="ou" />
        <property name="groupNameAttribute" value="cn" />
        <property name="groupMemberAttribute" value="member" />
      </bean>
    </beans>

The following is an example of how to configure the LDAP Identity Provider Plugin in bpm-platform.xml / processes.xml:

    <process-engine name="default">
      <job-acquisition>default</job-acquisition>
      <configuration>org.camunda.bpm.engine.impl.cfg.StandaloneProcessEngineConfiguration</configuration>
      <datasource>java:jdbc/ProcessEngine</datasource>

      <properties>...</properties>

      <plugins>
        <plugin>
          <class>org.camunda.bpm.identity.impl.ldap.plugin.LdapIdentityProviderPlugin</class>
          <properties>

            <property name="serverUrl">ldap://localhost:4334/</property>
            <property name="managerDn">uid=jonny,ou=office-berlin,o=camunda,c=org</property>
            <property name="managerPassword">s3cr3t</property>

            <property name="baseDn">o=camunda,c=org</property>

            <property name="userSearchBase"></property>
            <property name="userSearchFilter">(objectclass=person)</property>

            <property name="userIdAttribute">uid</property>
            <property name="userFirstnameAttribute">cn</property>
            <property name="userLastnameAttribute">sn</property>
            <property name="userEmailAttribute">mail</property>
            <property name="userPasswordAttribute">userpassword</property>

            <property name="groupSearchBase"></property>
            <property name="groupSearchFilter">(objectclass=groupOfNames)</property>
            <property name="groupIdAttribute">ou</property>
            <property name="groupNameAttribute">cn</property>

            <property name="groupMemberAttribute">member</property>

          </properties>
        </plugin>
      </plugins>

    </process-engine>

<div class="alert alert-info">
  <p>
    <strong>Administrator Authorization Plugin</strong>
    The LDAP Identity Provider Plugin is usually used in combination with the <a href="ref:#process-engine-authorization-service-the-administrator-authorization-plugin">Administrator Authorization Plugin</a> which allows you to grant administrator authorizations for a particular LDAP User / Group.
  </p>
</div>

The LDAP Identity Provider provides the following configuration properties:

<table class="table table-striped">
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>`serverUrl`</td>
    <td>The url of the LDAP server to connect to.</td>
  </tr>
  <tr>
    <td>`managerDn`</td>
    <td>The absolute DN of the manager user of the LDAP directory.</td>
  </tr>
  <tr>
    <td>`managerPassword`</td>
    <td>The password of the manager user of the LDAP directory</td>
  </tr>
  <tr>
    <td>`baseDn`</td>
    <td>
      <p>The base DN: identifies the root of the LDAP directory. Is appended to all DN names composed for searching for users or groups.</p>
      <p><em>Example:</em> `o=camunda,c=org`</p>
    </td>
  </tr>
  <tr>
    <td>`userSearchBase`</td>
    <td>
      <p>Identifies the node in the LDAP tree under which the plugin should search for users. Must be relative to `baseDn`.</p>
      <p><em>Example:</em> `ou=employees`</p>
    </td>
  </tr>
  <tr>
    <td>`userSearchFilter`</td>
    <td>
      <p>LDAP query string used when searching for users. <em>Example:</em> `(objectclass=person)`</p>
    </td>
  </tr>
  <tr>
    <td>`userIdAttribute`</td>
    <td>
      <p>Name of the user Id property. <em>Example:</em> `uid`</p>
    </td>
  </tr>
  <tr>
    <td>`userFirstnameAttribute`</td>
    <td>
      <p>Name of the firstname property. <em>Example:</em> `cn`</p>
    </td>
  </tr>
  <tr>
    <td>`userLastnameAttribute`</td>
    <td>
      <p>Name of the lastname property. <em>Example:</em> `sn`</p>
    </td>
  </tr>
  <tr>
    <td>`userEmailAttribute`</td>
    <td>
      <p>Name of the email property. <em>Example:</em> `mail`</p>
    </td>
  </tr>
  <tr>
    <td>`userPasswordAttribute`</td>
    <td>
      <p>Name of the password property. <em>Example:</em> `userpassword`</p>
    </td>
  </tr>
  <tr>
    <td>`groupSearchBase`</td>
    <td>
      <p>Identifies the node in the LDAP tree under which the plugin should search for groups. Must be relative to `baseDn`.</p>
      <p><em>Example:</em> `ou=roles`</p>
    </td>
  </tr>
  <tr>
    <td>`groupSearchFilter`</td>
    <td>
      <p>LDAP query string used when searching for groups. <em>Example:</em> `(objectclass=groupOfNames)`</p>
    </td>
  </tr>
  <tr>
    <td>`groupIdAttribute`</td>
    <td>
      <p>Name of the group Id property. <em>Example:</em> `ou`</p>
    </td>
  </tr>
  <tr>
    <td>`groupNameAttribute`</td>
    <td>
      <p>Name of the group Name property. <em>Example:</em> `cn`</p>
    </td>
  </tr>
  <tr>
    <td>`groupTypeAttribute`</td>
    <td><p>Name of the group Type property. <em>Example:</em> `cn`</p></td>
  </tr>
  <tr>
    <td>`groupMemberAttribute`</td>
    <td>
      <p>Name of the member attribute. <em>Example:</em> `member`</p>
    </td>
  </tr>
  <tr>
    <td>`acceptUntrustedCertificates`</td>
    <td>
      <p>Accept of untrusted certificates if LDAP server uses Ssl. <strong>Warning:</strong> we strongly advise against using this property. Better install untrusted certificates to JDK key store.</p>
    </td>
  </tr>
  <tr>
    <td>`useSsl`</td>
    <td>
      <p>Set to true if LDAP connection uses SSL. <em>Default:</em> `false`</p>
    </td>
  </tr>
  <tr>
    <td>`initialContextFactory`</td>
    <td>
      <p>Value for the `java.naming.factory.initial` property. <em>Default:</em> `com.sun.jndi.ldap.LdapCtxFactory`</p>
    </td>
  </tr>
  <tr>
    <td>`securityAuthentication`</td>
    <td>
      <p>Value for the `java.naming.security.authentication` property. <em>Default:</em> `simple`</p>
    </td>
  </tr>
</table>

