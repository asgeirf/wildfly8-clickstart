#  Wildfly clickstart

This clickstart sets up a build service, repository and a basic Wildfly app with continuous deployment.
All built by maven. 

<imc src="https://raw.github.com/CloudBees-community/wildfly8-clickstart/master/icon.png"/>

<a href="https://grandcentral.cloudbees.com/?CB_clickstart=https://raw.github.com/CloudBees-community/wildfly8-clickstart/master/clickstart.json"><img src="https://d3ko533tu1ozfq.cloudfront.net/clickstart/deployInstantly.png"/></a>

Launch this clickstart and glory could be yours too ! Use it as a building block if you like.
You can launch this on Cloudbees via a clickstart automatically, or follow the instructions below. 

This is based on the Wildfly stack that you can read more about <a href="https://github.com/CloudBees-community/wildfly8-clickstack">here.</a>


# Deploying manually: 


## Create a Wildfly8 container

```
bees app:create -a my-wildfly8-app -t wildfly8
```

## Create a MySQL Database

```
bees db:create my-wildfly8-db
```

## Bind the MySQL Database to the Wildfly container

```
bees app:bind -a my-wildfly8-app -db my-wildfly8-db -as mydb
```

Supported JNDI names:

 * `java:jboss/datasources/mydb` : qualified JNDI name is supported / **OK**

Samples:

```java
Context ctx = new InitialContext();
DataSource ds = (DataSource) ctx.lookup("java:jboss/datasources/mydb");
```

## Configure Jdbc Realm

```
bees config:set -a my-wildfly8-app -R wildfly8.auth-realm.database=mydb
```

## Deploy Application

```
bees app:deploy -a my-wildfly8-app path/to/my/app.war
```

## Jenkins Job

Create a new Maven project in Jenkins, changing the following:

* Add this git repository (or yours, with this code) on Jenkins
* Also check "Deploy to CloudBees" with those parameters:

        Applications: First Match
        Application Id: MYAPP_ID
        Filename Pattern: target/*.war

where `MYAPP_ID`is the name of your application.


