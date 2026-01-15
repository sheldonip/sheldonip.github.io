---
title: "Arranging Filters in Spring Boot"
date: 2026-01-14T23:38:00Z
draft: false
image: cover.jpg
---

## Introduction

As the web application grows, we may want to apply some logic before or after a servlet. For Spring Boot, filters would be a good option for us to store the logic. When a request is sent to the application, Spring Boot creates a set of filters as a filter chain for processing before invoking downstream filters and the servlet. 

That means the ordering of those filters is extremely important as the application grows. We are going to discuss how to configure the ordering to fulfil our goals.

## Scenario

For example, we have a web service protected by Spring Security. Only authorised users can access it. The service is running on Spring Boot 3.2.x and Spring Security 7.0.x. We now have a new and custom-made filter to log every incoming requests, including those unauthorised requests, for analytic purposes.

## Implementation

We should bear in mind that if the logging filter is invoked after the security filter chain (`SecurityFilterChain`). The logging filter would not be invoked if the incoming request is not authorised, and hence no custom logs for those requests.

We can change the order of the logging filter so that it would be invoked before the security filter chain. To do so, we need to set the order when the bean of the filter is created:

```java
registrationBean.setOrder(SecurityProperties.DEFAULT_FILTER_ORDER);
```

If we want the filter has a slightly higher priority than the security filter chain, we can also set:

```java
registrationBean.setOrder(SecurityProperties.DEFAULT_FILTER_ORDER - 1);
```

The lower value in the order means a higher priority, so the filter would be executed earlier in the filter chain.

This logging filter should be invoked before the security filter chain to generate custom logs for every authorised and unauthorised requests.

However, this configuration means invoking the code in the logging filter before Spring Security filters. We should be careful on what code would be executed in the filter to avoid security and privacy issues.


## Wrapping up

We have discussed how to configure the ordering of filters to invoke a filter before the security filter chain. We should be aware the potential security issues for this kind of configuration. ♠


## References

- https://docs.spring.io/spring-security/reference/servlet/architecture.html
- https://stackoverflow.com/questions/25957879/filter-order-in-spring-boot
- https://docs.spring.io/spring-security/site/docs/3.0.x/reference/security-filter-chain.html


## Credits

Cover photo by [Peter Bryan](https://unsplash.com/@peterbbryan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/a-close-up-of-a-clock-with-gears-attached-to-it-KORGgJI1odA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 
      