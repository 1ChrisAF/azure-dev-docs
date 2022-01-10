---
title: Deploy a Quarkus Web App to Azure App Service with Maven
description: Learn how to deploy a Quarkus App to App Service on Linux using the Maven Plugin for Azure Web App.
services: app-service
documentationcenter: java
ms.date: 06/10/2020
ms.service: app-service
ms.topic: article
ms.custom:   
#Customer intent: As a Java developer, I want to deploy MicroProfile apps to Azure so that I don't have to deal with app server configuration and management.
---

# Deploy a Quarkus Web App to Azure App Service with Maven

In this quickstart, you'll use the [Maven Plugin for Azure App Service Web Apps](https://github.com/microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) to deploy a Quarkus application to [Azure App Service on Linux](/azure/app-service/containers/). You'll want to choose Java SE deployment over [Tomcat and WAR files](/azure/app-service/containers/quickstart-java) when you want to consolidate your app's dependencies, runtime, and configuration into a single deployable artifact.

If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

## Prerequisites

* The [Azure CLI](/cli/azure/), either locally or through [Azure Cloud Shell](https://shell.azure.com).
* A supported Java Development Kit (JDK). For more information about the JDKs available for use when developing on Azure, see [Java support on Azure and Azure Stack](../fundamentals/java-support-on-azure.md).
* Apache [Maven](https://maven.apache.org/), version 3.

## Install and sign in to Azure CLI

The simplest and easiest way to get the Maven Plugin deploying your Quarkus application is by using [Azure CLI](/cli/azure/).

Sign into your Azure account by using the Azure CLI:

```azurecli
az login
```

Follow the instructions to complete the sign-in process.

## Create sample app from MicroProfile Starter

In this section, you'll create a Quarkus application and test it locally.

### Create Java SE 8 base Project

1. Open a web browser and navigate to the [MicroProfile Starter](https://start.microprofile.io/) site.

   ![MicroProfile Starter for Quarkus](./media/quarkus/microprofile-starter-quarkus.png)

2. Provide the following values for the indicated fields.

   |  Field  |  Value  |
   | ---- | ---- |
   |  groupId  |  com.microsoft.azure.samples.quarkus  |
   |  artifactId  |  quarkus-hello-azure  |
   |  MicroProfile Version  |  MP 3.2  |
   |  Java SE Version  |  Java 8  |
   |  MicroProfile Runtime  |  Quarkus  |
   |  Examples for Specifications  |  Metrics, OpenAPI  |

3. Select the **DOWNLOAD** button to download the project.

4. Unzip the archive file; for example:

   ```bash
   unzip Quarkus-hello-azure.zip
   ```

### Create Java SE 11 base Project

To create the Java 11 base project, use the following command:

   ```bash
   mvn io.quarkus:quarkus-maven-plugin:2.6.1.Final:create \
     -DprojectGroupId=com.microsoft.azure.samples.quarkus \
     -DprojectArtifactId=quarkus-hello-azure  \
     -DclassName="com.microsoft.azure.samples.quarkus.App" \
     -Dpath="/hello"
   ```

### Run the application in Local environment

1. Change directory to the completed project; for example:

   ```bash
   cd quarkus-hello-azure/
   ```

2. Build and Run the project using Maven; for example:

   ```bash
   mvn quarkus:dev
   ```

3. Test the web application by browsing to it locally using a web browser. For example, you could use the following command if you have curl available:

   For Java SE 8 Project:

   ```bash
   curl http://localhost:8080/data/hello
   ```

   For Java SE 11 Project:

   ```bash
   curl localhost:8080/hello
   ```

4. You should see the following message displayed: **Hello World** or **hello**

## Configure Maven Plugin for Azure App Service

In this section, you'll configure the Quarkus project *pom.xml* file so that Maven can deploy the app to Azure App Service on Linux.

1. Open the *pom.xml* file in a code editor.

2. In the `<build>` section of the *pom.xml* file, insert the following `<plugin>` entry inside the `<plugins>` tag after `maven-surefire-plugin`.

   ```xml
   <plugin>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-webapp-maven-plugin</artifactId>
     <version>2.3.0</version>
   </plugin>
   ```

3. To configure the deployment, run the following Maven command:

   ```bash
   mvn azure-webapp:config
   ```

   Select the following options when prompted:

   |  Input Field  |  Input/Select Value  |
   | ---- | ---- |
   |  Define value for OS(Default: Linux):  | 1. linux  |
   |  Define value for javaVersion(Default: Java 8):   | 1. Java 11  |
   |  Confirm (Y/N) | y |

   This produces output similar to the following example:

   ```output
   [INFO] Scanning for projects...
   [INFO] 
   [INFO] ------< com.microsoft.azure.samples.quarkus:quarkus-hello-azure >-------
   [INFO] Building quarkus-hello-azure 1.0-SNAPSHOT
   [INFO] --------------------------------[ jar ]---------------------------------
   [INFO] 
   [INFO] --- azure-webapp-maven-plugin:2.3.0:config (default-cli) @ quarkus-hello-azure ---

   Define value for OS(Default: Linux): 
   1. linux [*]
   2. windows
   3. docker
   Enter index to use: 
   Define value for javaVersion(Default: Java 8): 
   1. Java 11
   2. Java 8 [*]
   Enter index to use: 1
   Please confirm webapp properties
   AppName : quarkus-hello-azure-1601011883156
   ResourceGroup : quarkus-hello-azure-1601011883156-rg
   Region : westeurope
   PricingTier : PremiumV2_P1v2
   OS : Linux
   RuntimeStack : JAVA 11-java11
   Deploy to slot : false
   Confirm (Y/N)? : y
   [INFO] Saving configuration to pom.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  16.502 s
   [INFO] Finished at: 2020-09-25T14:31:34+09:00
   [INFO] ------------------------------------------------------------------------
   ```

4. Add the `<appSettings>` section to the `<configuration>` section of `PORT`, `WEBSITES_PORT` and `WEBSITES_CONTAINER_START_TIME_LIMIT`. The XML entry for `azure-webapp-maven-plugin` will look like the following example:

   ```xml
      <plugin>
        <groupId>com.microsoft.azure</groupId>  
        <artifactId>azure-webapp-maven-plugin</artifactId>  
        <version>2.3.0</version>  
        <configuration>
          <schemaVersion>V2</schemaVersion>
          <resourceGroup>microprofile</resourceGroup>
          <appName>quarkus-hello-azure-1591836715762</appName>
          <pricingTier>P1v2</pricingTier>
          <region>japaneast</region>
          <runtime>
            <os>linux</os>
            <javaVersion>java 11</javaVersion>
            <webContainer>java SE</webContainer>
          </runtime>
          <appSettings>
            <property>
              <name>PORT</name>
              <value>8080</value>
            </property>
            <property>
              <name>WEBSITES_PORT</name>
              <value>8080</value>
            </property>
            <property>
              <name>WEBSITES_CONTAINER_START_TIME_LIMIT</name>
              <value>600</value>
            </property>
          </appSettings>
          <deployment>
            <resources>
              <resource>
                <directory>${project.basedir}/target</directory>
                <includes>
                  <include>*.jar</include>
                </includes>
              </resource>
            </resources>
          </deployment>
        </configuration>
      </plugin>
   ```

5. Add the following entry to the *src/main/resources/application.properties* file to create the Uber (FAT) jar.

   ```properties
   quarkus.package.uber-jar=true
   ```

## Deploy the app to Azure

After you've configured all of the settings in the preceding sections of this article, you're ready to deploy your web application to Azure. To do so, use the following steps:

1. If you made any changes to the *pom.xml* file, rebuild the JAR file using the following command:

   ```bash
   mvn clean package
   ```

2. Deploy your web app to Azure by using the following command:

   ```bash
   mvn azure-webapp:deploy
   ```

   If the deployment succeeds, you'll see output similar to the following example:

   ```output
   [INFO] Successfully deployed the artifact to https://quarkus-hello-azure-1591836715762.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  02:20 min
   [INFO] Finished at: 2020-06-11T10:06:51+09:00
   [INFO] ------------------------------------------------------------------------
   ```

   Maven will deploy your web application to Azure. If the web application or web application plan does not already exist, it'll be created for you. It might take a few minutes before the web application is visible at the URL shown in the output. Navigate to the URL in a web browser. You should see the following screen.

   ![Front Page for Quarkus](./media/quarkus/quarkus-front-page-11.png)

   When your web application has been deployed, you'll be able to manage it through the [Azure portal].

   * Your web application will be listed in **microprofile** resource group:

   ![Web app listed in Azure portal App Services](./media/quarkus/quarkus-azure-portal-rg.png)

   * And you can access to your web application by clicking the `Browse` button in the **Overview** for your web app.  
   Verify that the deployment was successful and is running. You should see the following screen displayed:

   ![Find the URL for your web app in Azure portal App Services](./media/quarkus/quarkus-azure-portal-manage.png)

## Confirm the Log Stream from Running App Service

You can see (or "tail") the logs from the running App Service. Any calls to `console.log` in the site code are displayed in the terminal.

   ```azurecli
   az webapp log tail -g microprofile -n quarkus-hello-azure-1601011883156
   ```

   ![Terminal window showing log output.](./media/Quarkus/azure-cli-app-service-log-stream.png)

## Clean up Resources

When the Azure resources are no longer needed, clean up the resources you deployed by deleting the resource group.

* From the Azure portal, select Resource group from the left menu.
* Enter **microprofile** in the **Filter by name** field, the resource group created in this tutorial should have this prefix.
* Select the resource group created in this tutorial.
* Select Delete resource group from the top menu.

## Next steps

To learn more about MicroProfile and Azure, continue to the MicroProfile on Azure documentation center.

> [!div class="nextstepaction"]
> [MicroProfile on Azure](./index.yml)

### Additional Resources

For more information about the various technologies discussed in this article, see the following articles:

* [Maven Plugin for Azure Web Apps]

* [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven Settings Reference](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: ../index.yml
[Azure portal]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Maven Plugin for Azure Web Apps]: https://github.com/microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md

[Java Development Kit (JDK)]: ../fundamentals/java-support-on-azure.md
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: media/deploy-spring-boot-java-app-with-maven-plugin/web-app-listed-azure-portal.png
[AP02]: media/deploy-spring-boot-java-app-with-maven-plugin/determine-web-app-url.png
