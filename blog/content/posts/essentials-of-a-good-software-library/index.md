---
title: "Essentials of a Good Software Library"
date: 2025-08-30T04:45:00Z
draft: false
image: cover.jpg
---

## Introduction

Sometimes we look for a software library before writing the implementation on our own to avoid reinventing the wheel, like this [stackoverflow](https://stackoverflow.com/questions/1236256/how-do-you-decide-whether-to-use-a-library-or-write-your-own-implementation) question. We are going to talk about what makes a software library good enough for us to pick.

## Criteria

### License

Generally we would like to pick a library with an open source license to make sure it can be freely used, modified and shared. This saves us a lot of costs.

### Functionality

After making sure we can use the library, we probably want to check that library exposes functions that we would like to use. We can conduct tests to verify its output is as expected. It will also be great if the library is well-tested and checked automatically in its CI/CD pipeline.

### Environment

If a library is not designed for the programming language we use or it ties to a specific environment, it can be a good reference for us to implement our own library rather than use it directly and increasing complexity of our software.

### Document

A good library should comes with a document showing detailed installation steps. It should be easy to install. The document should give examples to show how to use in different scenarios, which helps a lot on understanding how the library behaves. The document should also shows how to customise the library to meet different needs.

### Dependency

Security and performance on the dependencies of the library are another concerns. We do not want to accidentally introduce vulnerabilities to our software. We also do not want the library requires unreasonable size or scope of dependencies to make it works.

### Versioning

Of course a library should follow semantic versioning. It should be backward compatible when upgrading a minor version. This assures a manageable migration, enabling a long term use.

### Speed and Weight

A good library should be fast and lightweight. We do not want the library becomes a bottleneck of our software. We do not want to lose the control of our software because of this. Light weight means we have more room for our code while keeping a reasonable size.

### Development

Apart from documentation, a good library should be easy to extend ideally. It will also be great if it is easy to give feedback to the owners. In this way, we can contribute back to the library, keeping the library actively maintained.

## Wrapping up

We have gone through the essentials of a good software library. We are now aware of the license, functionalities, environment and other criteria when choosing a software library. As always, the world is changing fast and this list should have changes after some time.

## References

- https://opensource.org/licenses
- https://stackoverflow.com/questions/1236256/how-do-you-decide-whether-to-use-a-library-or-write-your-own-implementation
- https://semver.org/


## Credits

Cover photo by [Yasamine June](https://unsplash.com/@yasamine?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-close-up-of-a-mouse-on-a-table-SLipZqBFLHU?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 
