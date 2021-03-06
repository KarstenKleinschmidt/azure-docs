---
title: "Quickstart: Computer Vision client library for Java"
titleSuffix: "Azure Cognitive Services"
description: Get started with the Computer Vision client library for Java.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: 
ms.topic: quickstart
ms.date: 07/25/2019
ms.author: pafarley
---

# Quickstart: Computer Vision client library for Java

Get started with the Computer Vision client library for Java. Follow these steps to install the package and try out the example code for basic tasks. Computer Vision provides you with access to advanced algorithms for processing images and returning information.

Use the Computer Vision client library for Java to:

* Analyze an image for tags, text description, faces, adult content, and more.

[Reference documentation](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/computervision?view=azure-java-stable) | [Artifact (Maven)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) | [Samples](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## Prerequisites

* Azure subscription - [Create one for free](https://azure.microsoft.com/free/)
* The current version of the [Java Development Kit(JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* The [Gradle build tool](https://gradle.org/install/), or another dependency manager.

## Setting up

### Create a Computer Vision Azure resource

Azure Cognitive Services are represented by Azure resources that you subscribe to. Create a resource for Computer Vision using the [Azure portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) or [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) on your local machine. You can also:

* Get a [trial key](https://azure.microsoft.com/try/cognitive-services/#decision) valid for seven days for free. After you sign up, it will be available on the [Azure website](https://azure.microsoft.com/try/cognitive-services/my-apis/).  
* View your resource on the [Azure portal](https://portal.azure.com/).

Then, [create environment variables](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) for the key and service endpoint string, named `COMPUTER_VISION_SUBSCRIPTION_KEY` and `COMPUTER_VISION_ENDPOINT`, respectively.

### Create a new Gradle project

In a console window (such as cmd, PowerShell, or Bash), create a new directory for your app, and navigate to it. 

```console
mkdir myapp && cd myapp
```

Run the `gradle init` command from your working directory. This command will create essential build files for Gradle, including *build.gradle.kts*, which is used at runtime to create and configure your application.

```console
gradle init --type basic
```

When prompted to choose a **DSL**, select **Kotlin**.

Locate *build.gradle.kts* and open it with your preferred IDE or text editor. Then copy in the following build configuration. This configuration defines the project as a Java application whose entry point is the class **ComputerVisionQuickstarts**. It imports the Computer Vision library.

```kotlin
plugins {
    java
    application
}
application { 
    mainClassName = "ComputerVisionQuickstarts"
}
repositories {
    mavenCentral()
}
```

From your working directory, run the following command to create a project source folder:

```console
mkdir -p src/main/java
```

Navigate to the new folder and create a file called *ComputerVisionQuickstarts.java*. Open it in your preferred editor or IDE and add the following `import` statements:

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_imports)]

Then add a class definition for **ComputerVisionQuickstarts**.

### Install the client library

This quickstart uses the Gradle dependency manager. You can find the client library and information for other dependency managers on the [Maven Central Repository](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-textanalytics/).

In your project's *build.gradle.kts* file, include the Computer Vision client library as a dependency.

```kotlin
dependencies {
	compile(group = "com.microsoft.azure.cognitiveservices", name = "azure-cognitiveservices-computervision", version = "1.0.2-beta")
}
```

## Object model

The following classes and interfaces handle some of the major features of the Computer Vision Java SDK.

|Name|Description|
|---|---|
| [ComputerVisionClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-java-stable) | This class is needed for all Computer Vision functionality. You instantiate it with your subscription information, and you use it to produce instances of other classes.|
|[ComputerVision](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervision?view=azure-java-stable)| This class comes from the client object and directly handles all of the image operations, such as image analysis, text detection, and thumbnail generation.
|[VisualFeatureTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-java-stable)| This enum defines the different types of image analysis that can be done in a standard Analyze operation. You specify a set of VisualFeatureTypes values depending on your needs. |

## Code examples

These code snippets show you how to do the following tasks with the Computer Vision client library for Java:

* [Authenticate the client](#authenticate-the-client)
* [Analyze an image](#analyze-an-image)

## Authenticate the client

> [!NOTE]
> This quickstart assumes you've [created an environment variable](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) for your Computer Vision key, named `COMPUTER_VISION_SUBSCRIPTION_KEY`.

The following code adds a `main` method to your class and creates variables for your resource's Azure endpoint and key. You'll need to enter your own endpoint string, which you can find by checking the **Overview** section of the Azure portal. 

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_mainvars)]

Next, add the following code to create a [ComputerVisionClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-java-stable) object and passes it into other method(s), which you'll define later.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_client)]

> [!NOTE]
> If you created the environment variable after you launched the application, you'll need to close and reopen the editor, IDE, or shell running it to access the variable.

## Analyze an image

The following code defines a method, `AnalyzeLocalImage`, which uses the client object to analyze a local image and print the results. The method returns a text description, categorization, list of tags, detected faces, adult content flags, main colors, and image type.

### Set up test image

First, create a **resources/** folder in the **src/main/** folder of your project, and add an image you'd like to analyze. Then add the following method definition to your **ComputerVisionQuickstarts** class. If necessary, change the value of the `pathToLocalImage` to match your image file. 

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_analyzelocal_refs)]

### Specify visual features

Next, specify which visual features you'd like to extract in your analysis. See the [VisualFeatureTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-java-stable) enum for a complete list.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_analyzelocal_features)]

### Analyze
This method prints detailed results to the console for each scope of image analysis. We recommend you surround this method call in a Try/Catch block

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_analyzelocal_analyze)]

### Display results

The above method call will return an ImageAnalysis object that contains all of extracted information. You can use a code block like the following to print the details of a given visual feature.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_analyzelocal_display)]

See the sample code on [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/ComputerVisionQuickstart.java) for a full set of display options.

## Run the application

You can build the app with:

```console
gradle build
```

Run the application with the `gradle run` command:

```console
gradle run
```

## Clean up resources

If you want to clean up and remove a Cognitive Services subscription, you can delete the resource or resource group. Deleting the resource group also deletes any other resources associated with it.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## Next steps

In this quickstart, you learned how to use the Computer Vision Java library to do basis tasks. Next, explore the reference documentation to learn more about the library.

> [!div class="nextstepaction"]
>[Computer Vision reference (Java)](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/computervision?view=azure-java-stable)

* [What is Computer Vision?](../Home.md)
* The source code for this sample can be found on [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/ComputerVisionQuickstart.java).