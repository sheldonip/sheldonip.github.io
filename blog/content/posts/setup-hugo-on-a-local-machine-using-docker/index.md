---
title: "Setup HUGO on a local machine using Docker"
date: 2023-08-25T18:00:00Z
draft: false
image: cover.jpg
---

## Introduction

This post is sharing how to setup HUGO on a local machine using Docker Desktop. We will also try to create a simple page and a post.

## Shall we start?
We are using 0.107.0 version of HUGO here. But you may want to check docker hub for other stable version number.

In an empty folder, we are going to create a new site called `new-site`, execute:

```bash {linenos=false}
docker run --rm -v $(pwd):/src klakegg/hugo:0.107.0-ext-alpine new site new-site --format yaml
```

A folder name `new-site` should be generated. 

Create `docker-compose.yml` in the root of the project:
```yml
services:
  server:
      image: klakegg/hugo:0.107.0-ext-alpine
      command: server -D --poll 700ms
      volumes:
        - "./new-site:/src"
      ports:
        - "1313:1313"
```
We have added `-D --poll 700ms` parameters to include drafts for an easier development on our local machine.

Execute the following to serve the site:
```bash
docker-compose up
```

Navigate to [http://localhost:1313](http://localhost:1313), you should be seeing a “Page not found” message as there is no content in the site.

Now, we try to add content. Create `layouts/index.html`:
```html
<!doctype html> 
<html lang="en"> 
  <head> 
    <meta charset="utf-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1"> 
    <title>HUGO & Docker demo</title>  
  </head> 
  <body> 
    <h1>Hello, world!!!</h1> 
  </body> 
</html>
```

Navigate to [http://localhost:1313](http://localhost:1313) again and you should be seeing “Hello, world !!!” message.

### Basic Configuration
We are now setting the production url, language and the titel of the new site. Create `config.toml` in the `new-site` directory:
```toml
baseURL = 'http://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
```
The value of `baseURL` must start with the protocol and end with a slash.

### Writing a post
To add a new post, execute the following in the container:
```bash
hugo new posts/my-first-post.md
```

Open the file generated in `/content/posts/my-first-post.md`, you should see something like this:
```markdown
---
title: "My First Post"
date: 2023-08-19T15:27:16Z
draft: true
---
```

Add some markdown to the body of the post, but do not change the `draft` value. For example:

```markdown
---
title: "My First Post"
date: 2023-08-19T15:27:16Z
draft: true
---

## Introduction

This is **bold** text, and this is *emphasized* text.

Visit the [Hugo](https://gohugo.io) website!
```

Navigate to [`http://localhost:1313/posts/my-first-post/`](http://localhost:1313/posts/my-first-post/) . The site should be rebuilt automatically. You should be seeing the content you just added.

### Publish the post
Update the value of `draft` from `true` to `false` in the markdown file, then execute:
```bash
hugo
```
This will build and publish the site, you can view the published files under `public/`. To publish again, we have to manually delete everything under `public/` before execute the above command again.

## Wrapping up

We have setup a HUGO site using Docker and Docker compose on a local machine. We also created an index page and published a post.


## References
- [`Setup HUGO using Docker - DEV Community`](https://dev.to/robinvanderknaap/setup-hugo-using-docker-43pm)
- [`klakegg/docker-hugo: Truly minimal Docker images for Hugo open-source static site generator.`](https://github.com/klakegg/docker-hugo)
- [`Quick start | Hugo`](https://gohugo.io/getting-started/quick-start/)
- [`Content organization | Hugo`](https://gohugo.io/content-management/organization/)
