---
title: "Utilising Tess4J 5.15 with Gradle"
date: 2025-06-03T21:01:00Z
draft: false
image: cover.jpg
---

## Introduction

Tess4J is a java wrapper for Tesseract, which is a great optical character recognition (OCR) library for processing images and pdf files. It is handy to use from command line but we may need extra steps to setup before utilising it from an IDE. Otherwise, we may encounter an error message like: `java.lang.UnsatisfiedLinkError: Unable to load library`.

## Implementation

We are going to resolve the `UnsatisfiedLinkError`. We assumed Tesseract 5.5.x is installed via brew in macOS:

```bash
brew install tesseract
```

Tess4J 5.15.x is imported into a gradle project by `build.gradle.kts`:

```kotlin
implementation("net.sourceforge.tess4j:tess4j:5.15.0")
```

We set the data path of tesseract to the `tessdata` folder in our machine.

However, if we try to run a function with Tess4J, the IDE would show the `UnsatisfiedLinkError`. After diving in, we find out that we have to load two libraries: `libtesseract.dylib` and `libleptonica.dylib` to make it works.

We try to import those libraries from installed Tesseract 5.5.x to Tess4J jar file through command line:

1. Find out where the `tess4j-5.15.0.jar` resides and go to that directory, in gradle's case, this should be: `cd /Users/<YOUR_USERNAME>/.gradle/caches/modules-2/files-2.1/net.sourceforge.tess4j/tess4j/5.15.0/<HASH>`
2. `mkdir darwin`
3. `jar uf tess4j-5.15.0.jar darwin`
4. `cp /usr/local/Cellar/tesseract/5.5.1/lib/libtesseract.5.dylib darwin/libtesseract.dylib`
5. `jar uf tess4j-5.15.0.jar darwin/libtesseract.dylib`
6. `cp /usr/local/Cellar/leptonica/1.85.0/lib/libleptonica.6.dylib darwin/libleptonica.dylib`
7. `jar uf tess4j-5.15.0.jar darwin/libleptonica.dylib`

The two libraries should be imported to the jar. We can verify by: `jar tf tess4j-5.15.0.jar` and the end of the output should be:

```bash
darwin/
darwin/libtesseract.dylib
darwin/libleptonica.dylib
```

We can now go back to our IDE and run the function. It should work without the error.

## Wrapping up

We have done a manual import for Tess4J to make it works with our IDE. We can now use Tesseract to perform OCR for processing images and pdf files through our IDE. For those using maven, you may want to take a look on the stackoverflow link in the reference section to resolve the error. 


## References

- https://stackoverflow.com/questions/21394537/tess4j-unsatisfied-link-error-on-mac-os-x


## Credits

Cover photo by [Red Mirror](https://unsplash.com/@redmirror?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/wall-with-paints-f303VzauP6w?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 

