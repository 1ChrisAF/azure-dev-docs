---
title: "Step 3: Create the App Service from Visual Studio Code"
description: Tutorial step 3, creating the App Service from the VS Code extension.
ms.topic: conceptual
ms.date: 07/16/2021
ms.custom: devx-track-python, seo-python-october2019
---

# 3: Create the App Service from Visual Studio Code

[Previous step: prepare your app](tutorial-deploy-app-service-on-linux-02.md)

In this step, you create an **App Service web app** to which you deploy your app. In App Service terminology, a "web app" is a **host** for web app code, not the app code itself.

You do this step before deploying your code so you can configure a custom startup file in step 4.

1. In the **Azure** extension, under **RESOURCES**, select **+** to create a new App Service:

    ![Create new App Service in the App Service explorer](media/deploy-azure/create-new-app-service-in-app-service-explorer.png)

    You can alternately, expand your subscription and find the **App Services** extension. Right-click and select **Azure App Service: Create New Web App** command from the VS Code Command Palette.

    If you want to control the Azure region (location) in which you create the web app, select instead  the **Azure App Service: Create New Web App...(Advanced)** command from the Command Palette.

1. In the prompts that follow:

    - Enter a globally unique name for your app. To ensure uniqueness, you can use your name or company name followed by the app name.
    - Select **Python 3.9** as the runtime stack.
    - Select **Basic (B1)** for the pricing tier, which provides a good balance between cost and performance for development work.

    If you're using the Advanced option create the App Service, select **Create new resource group** after entering the app name, followed by a name for the group. After selecting the **Python 3.9** runtime stack, select an Azure location. Then follow the prompts to create an App Service Plan before selecting the pricing tier. You can skip creating an Application Insights resource for this tutorial.

1. When a message appears indicating that the new App Service was created, select **View Output** to switch to the **Output** window in VS Code:

    ![VS Code message with the URL for for your App Service](media/deploy-azure/url-for-your-new-app-service-and-resource-group-and-plan.png)

1. To confirm that the App Service is running properly, expand your subscription in the **Azure: App Service** explorer, right-click the App Service name, and select **Browse website**:

    ![Browse Website command on an App Service in the App Service explorer](media/deploy-azure/select-command-to-browse-website-in-app-service.png)

1. Because you haven't deployed your own code to the App Service yet (which you do in the next step), only a default app appears:

    ![Default Python app on App Service on Linux](media/deploy-azure/default-python-app-on-app-service-on-linux.png)

---

## (Optional) Upload an environment variable definitions file

If you have an environment variable definitions file, you can use that file to configure the App Service environment as well. (To learn more about such files, which typically have the *.env* extension, refer to [Visual Studio Code - Python Environments](https://code.visualstudio.com/docs/python/environments#_environment-variable-definitions-file).)

1. In the **Azure: App Service** extension, expand the node for the desired App Service, then right-click the **Application Settings** node and select **Upload Local Settings**.

1. VS Code prompts you for the location of your *.env* file, then uploads it to the App Service.

1. Once the upload is complete, you can expand the **Application Settings** node to see the individual values. You can also view them on the Azure portal by navigating to the App Service and selecting **Configuration**.

    These Application settings appear to your code as environment variables.

> [!TIP]
> If you create settings directly on the Azure portal, you can save them in a definitions file by right-clicking the **Application Settings** node and selecting **Download Remote Settings**. This process makes sure that you have those settings in your repository and not only on the portal.

> [!div class="nextstepaction"]
> [I created the App Service - continue to step 4 >>>](tutorial-deploy-app-service-on-linux-04.md)

[Having issues? Let us know.](https://aka.ms/FlaskVSCQuickstartHelp)
