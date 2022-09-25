# How to build the SSR web application and mobile apps from the same code base

Server-side rendering improves initial page load speed. The browser receives pre-rendered HTML from the server, so it takes less time to build the page.

Also, SSR sites play better with SEO. Search engine crawlers grab the site's HTML content without executing JS code. Without SSR, search engines will not index page content and will not find internal links.

NuxtJS has a [Server-side rendering](https://nuxtjs.org/docs/concepts/server-side-rendering/) feature:

> Server-side rendering (SSR), is the ability of an application to contribute by displaying the web-page on the server instead of rendering it in the browser. Server-side sends a fully rendered page to the client.

But you cannot use SSR when you build a static site for the mobile application. Here I'll show how to create an SSR web and cross-platform mobile application from the same code base using CapacitorJS and NuxtJS.

## Building the app

As an example, we will build a simple app to show local time in [Kyiv](https://en.wikipedia.org/wiki/Kyiv). 

To test SSR, we need to get data from the API server. We will receive data from [worldtimeapi.org](http://worldtimeapi.org/) API using the [useFetch](https://v3.nuxtjs.org/getting-started/data-fetching/#usefetch) method.

Let's start with the application from the [previous guide](https://dev.to/daiquiri_team/how-to-create-android-and-ios-apps-from-the-nuxtjs-application-using-capacitorjs-134h).

Remove `app.vue` file and add `pages/index.vue` with the following content:

```vue
<template>
  <div>
    <h1>🇺🇦 Kyiv Time</h1>

    <p>
      🕥 Current time: <b>{{ timeData.datetime }}</b>
      <a @click="$nuxt.refresh()" href="">🔄</a>
    </p>
  </div>
</template>

<script setup>
const { data: timeData } = await useFetch('https://worldtimeapi.org/api/timezone/Europe/Kiev')

</script>
```

Here we have:

- Retrieving current time in `const timeData` variable.
- 🕥 displaying time on the page.
- 🔄 refresh button.

Run `yarn dev` and check [http://localhost:3000/](http://localhost:3000/) what we have:

![](https://raw.githubusercontent.com/eugen1j/nuxt-mobile/main/blog/img/02_site_ssr_html.png)

Let's look at the page source code:

![](https://raw.githubusercontent.com/eugen1j/nuxt-mobile/main/blog/img/02_site_ssr_source.png)

In NuxtJS, Server-side rendering is enabled by default. But what will we get if we create a static site for the mobile build? Run `yarn generate && yarn preview` and recheck [http://localhost:3000/](http://localhost:3000/). 

If you try to refresh the page, you will see that time doesn't change. NuxtJS retrieved time from the server during static site generation and saved it in the pre-rendered HTML page.

Let's add `ssr: false` to the `nuxt.config.ts` 

```typescript
import { defineNuxtConfig } from 'nuxt'

// https://v3.nuxtjs.org/api/configuration/nuxt.config
export default defineNuxtConfig({
    ssr: false,
})
```

and rebuild the site with `yarn generate && yarn preview`.

Now NuxtJS runs HTTP requests on the client side. There is no datetime in the page sources, and datetime changes every time you refresh a page.

![](https://raw.githubusercontent.com/eugen1j/nuxt-mobile/main/blog/img/02_site_no_ssr_source.png)


So, we must build a web application with SSR and a mobile application in client-only mode. Let's use an environment flag `MOBILE_BUILD`: we will build a web application with `MOBILE_BUILD=0` and a mobile application with `MOBILE_BUILD=1`.

You can use a `.env` file to store environment variables, and I'll go with `export MOBILE_BUILD=1`.

Change `nuxt.config.ts`:

```typescript
import { defineNuxtConfig } from 'nuxt'

// https://v3.nuxtjs.org/api/configuration/nuxt.config
export default defineNuxtConfig({
    ssr: !process.env.MOBILE_BUILD,
})
```

Run `yarn generate && yarn preview` and verify that the application works in client-only mode.

Finally, let's build and check the Android application:

```
yarn cap sync
yarn cap open android
```
Run the application on an emulator and check that the refresh button works.

![](https://raw.githubusercontent.com/eugen1j/nuxt-mobile/main/blog/img/02_android.png)

🎉 Congratulations, it works (on my laptop, at least 🙂).
