---
title: "Gzip vs zstd Compression for Docker Images"
date: 2026-09-16T23:19:00Z
draft: false
image: cover.jpg
---

## Introduction

We use Docker to pack our applications and unpack them for deployment. We can optimise the cost of this process by selecting a different compression algorithm when building an image. Before making the decision, it is important for us to understand the difference of gzip and Zstandard (zstd) algorithms.

## Gzip

gzip is a well-known and reliable compression algorithm with a long history. It is also used in HTTP compression to improve sending packets on the internet. When we use `docker build` to compress and create an image, gzip is used as the default algorithm.

## Zstandard (zstd)

Zstd is a rather new compression algorithm developed in the 21st century. It is designed to achieve a faster decompression without losing data. To use it for building images in Docker, we have to add `compression=zstd` parameter in the command, along with `compression-level=<value>` if we want to use a specific compression level.

## Comparison

Docker supports gzip compression and decompressing gzipped images in all of its versions. Instead, for decompressing a zstd compressed image, we need Docker version 23.0 or newer to do so. When we need to support a wider range of clients, gzip is a better option.

For images with larger sizes, such as embedded LLM and AI models, the size and the time of compression are significant to the cost. That is where zstd shines. It can further reduce around 20% of the size compared to gzip in Docker 24 according to [the official figure](https://www.docker.com/blog/dockers-developer-innovation-unveiling-performance-milestones/). It is also able to consume 50% less time during compression and decompression according to [the test conducted by depot](https://depot.dev/blog/building-images-gzip-vs-zstd). And hence zstd reduces the times consumed in CI/CD pipelines.


## Wrapping up

We have gone through the strengths and weaknesses of the two algorithms. Both of them works well and data lossless. They are best suited for different scenarios. We should always understand the tradeoff before switching to another compression algorithm. ♠


## References

- https://docs.docker.com/build/exporters/
- https://en.wikipedia.org/wiki/Gzip
- https://en.wikipedia.org/wiki/Zstd
- https://aws.amazon.com/blogs/containers/reducing-aws-fargate-startup-times-with-zstd-compressed-container-images/
- https://docs.docker.com/engine/release-notes/23.0/
- https://depot.dev/blog/building-images-gzip-vs-zstd
- https://www.docker.com/blog/dockers-developer-innovation-unveiling-performance-milestones/


## Credits

Cover photo by [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/assorted-belts-and-packs-on-metal-case-GNQV8xrXUis?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 