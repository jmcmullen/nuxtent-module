# Nuxt-Content

`nuxt-content` facilitates the usage of markdown files in content heavy sites.
It does this first, by compiling all the data from markdown files based on configured rules and second, by providing helper methods for dynamically accessing this data inside Nuxt pages.

Best of all, `nuxt-content` is a lightweight abstraction that does as little work as possible. Nuxt is already great, so we only need to add a little bit of sugar on top to handle the content. :)

## Installation

```
npm install nuxt-content

```

Then, under `nuxt.config.js` install the module:

```
modules: [
   '@nuxtjs/content'
 ]
```

## Usage

There are two main steps to using `nuxt-content`: 1) configuring how you want your content data compiled and 2) dynamically using the content data inside Nuxt pages.

### Configuration

#### Directory Options

All content options can be configured either inside a `nuxt.content.js` file or under the `content` property in `nuxt.config.js`.

*Note: All paths are relative to Nuxt Source Directory.*

- `srcDir`, String that specifies the directory where the content is located. By default, all content is placed in the `/content` directory.
- `routeName`, String that specifies the name of the dynamic page route that serves as the content's page. This is necessary so that the route path can be changed to match the content's permalink configuration.
- `permalink`, String that specifies url path configuration options. The possible options are `:slug`, `:section`, `:year`, `:month`, `:day`.
- `isPost`, Boolean that specifies whether the content requires a date. The default is true.
- `data`, Object that specifies that additional data that you would like injected into your content's component.
- `dirs`, Array that specifies options for all content under a directory. A 2D array is also allowed to configure multiple content types. These nested configurations will override any global directory options mentioned above.

Here's an example:

```js
content: {
 // Global Options
 srcDir: "content" // default
 // Directory Options
 dirs: [
  // content/blog/2013-01-10-HelloWorld.md -> localhost:3000/2013/hello-world
  ["posts", {
    routeName: "post", // pages/_post.vue
    permalink: ':year/:slug',
    data: {
      author: "Alid Castano"
    }
  }],
  // content/projects/NuxtContent.md -> localhost:3000/projects/nuxt-content
  ["projects", {
    routeName: "projects-slug", // pages/projects/_slug.vue
     permalink: "projects/:slug",
    isPost: false
  }]
 ]
}

```

A couple considerations to keep in mind from the options above:

- For the `routeName` you have to convert a route's directory path to it's name. As a general rule, just ignore all initial underscores and file extensions, and separate the remaining words by a hypen.
- Since the route's will be changed to the content's `permalink`, be extra mindful of how you configure permalinks to avoid conflict. For example, `:year/:slug` and `:section/:slug` both conflict since you can only have one root level dynamic page. To avoid this, hard code sections whenever possible: `someSection/:slug` and `:year/slug`.
