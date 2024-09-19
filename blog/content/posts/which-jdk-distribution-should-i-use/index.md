---
title: "Which JDK distribution should I use?"
date: 2024-09-18T23:10:00Z
draft: false
image: cover.jpg
---

## Introduction
As the OpenJDK is available for organisations to publish their own builds for Java applications. There are various of binary distributions of JDK from different providers available for everyone. We are going to discuss some of the distributions in short to have a better image of them.

## Comparison

### [Oracle JDK](https://www.oracle.com/java/technologies/downloads/)
It is provided by Oracle with the official support from the company. It would be a great choice if you have consulted your lawyer and are willing to pay for the commerical environment.

### [OpenJDK](https://jdk.java.net/)
It is provided by Oracle as well without commercial support. It is open-source. However, those builds, including LTS versions, are updated only for 6 months. This maybe too short for a production environment.

### [Amazon Corretto](https://aws.amazon.com/corretto/)
It is a production-ready distribution by Amazon, optimised for Amazon Linux 2. It receives a long-term support from Amazon. It is a good and obvious choice for a production environment running in AWS.

### [Microsoft Build](https://learn.microsoft.com/en-us/java/openjdk/download)
It is a production-ready distribution by Microsoft. It may contains fixes and enhancements from the company. The LTS versions are quarterly updated. It receives a commerical support for those Azure customers subscribed with support plans on specific services. It maybe a good choice if you look for their official support particularly for the supported services.

### AdoptOpenJDK
It is moved to eclipse adoptium in 2021. You should consider to migrate as soon as possible.

### [Eclipse Adoptium](https://adoptium.net/marketplace/)
It is a community-driven and open-source distribution. It is production-ready. Obviously it does not have option of official commercial support from the working group. It is a good choice for a production environment.

### [Azul Zulu](https://www.azul.com/downloads/)
It is a production-ready and open-source distribution by Azul. It is officially supported in the Azul platform core and Azul platform prime. It is a good choice for a production environment.

### [BellSoft Liberica](https://bell-sw.com/pages/downloads/)
It is a production-ready and open-source distribution by BellSoft. It comes with options for commerical support from the company. It is also recommended by Spring Boot, which used as the default choice of JDK. Its container image is light-weight compared to the images installed with order JDK. It is a good choice for a production environment, especially running with Spring Boot applications.

### [GraalVM](https://www.graalvm.org/downloads/)
It is an open-source distribution developed by Oracle. It is added with extra features compared with the base JDK, such as a new high-performance compiler and a new polyglot virtual machine. One of its target is to execute code from any programming language in a single application. It can probably further reduce the startup time of microservices with proper adoption. It maybe a good choice for a highly performance-sensitive applications.

## Wrapping up
We have compared some of the major distributions of OpenJDK and understood what they are good at. For other distributions not mentioned above, you may want to check this [site](https://whichjdk.com/) out before using it.

As always, there are some trade-offs of each distribution and we should know before adapting them at scale.


## References

- https://whichjdk.com/
- https://medium.com/@javachampions/java-is-still-free-3-0-0-ocrt-2021-bca75c88d23b
- https://blog.jetbrains.com/idea/2024/05/java-runtimes-insights-from-the-spring-boot-point-of-view/
- https://en.wikipedia.org/wiki/OpenJDK
- https://en.wikipedia.org/wiki/GraalVM 

## Credits

Cover photo by [Lisheng Chang](https://unsplash.com/@changlisheng?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/aerial-photography-of-colorful-tent-M2524ncJQ40?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 
  