

# How to create Android and iOS apps from NuxtJS application

If you have a NuxtJS web app, and you need to have some native features or use AppStore and PlayMarket as a distribution channels, you can use CapacitorJS to turn it to Android and iOS applications.


## Install Nuxt 

If you already have a NuxtJS web application you can jump to the [Add CapacitorJS](#Add CapacitorJS) section

Installing nuxt3 https://v3.nuxtjs.org/getting-started/quick-start

```
$ npx nuxi init nuxt-mobile
$ cd nuxt-mobile
$ yarn install
```

Let's change `app.vue`

```html
<template>
  <div>
    <h1>Nuxt Mobile</h1>

    <p>This is the very basic example of mobile app built with NuxtJS and CapacitorJS</p>
  </div>
</template>
```

```
$ yarn dev -o
```


![](img/01_nuxt_web.png)


Look at the browser.

## Add CapacitorJS

Add capacitor https://capacitorjs.com/docs/getting-started

```
$ yarn add @capacitor/core
$ yarn add --dev @capacitor/cli
```

Initialize proj 

```
$ yarn cap init
```

I'll pick Name: NuxtMobile; Package:  com.mycompany.nuxtmobile;  Web asset directory … dist

Check `capacitor.config.json`


## Add Android and iOS packages

```
$ yarn add @capacitor/android @capacitor/ios
```

We need a static site to build mobile app. So, run 

```
$ yarn generate 
```

Now we have static site, so we can init ios and android app

```
$ yarn cap add android
$ yarn cap add ios
```

We created folders `android` and `ios` with native apps. 

If you want to change site, you need to run `yarn generate` to generate a static site and `npx cap sync` update native apps.


## Running Android app

You can run Android on any OS.

Install https://developer.android.com/studio.

Run `yarn cap open android`. This will open AndroidStudio.

We need to add an emulator to run. Go to the Tools -> Device Manager, click Create Device. 

First, you need to pick Hardware device. I'll go with `Pixel 2`. 

![](img/01_android_emulator_1.png)

Then, you need to download and pick Android OS image. I will use `API 33`. 

![](img/01_android_emulator_2.png)

Then click 'Next' and 'Finish'.

After this you will see a new device on the 'Devices' dropdown. Now, you can click 'Run' to build and start application:

![](img/01_andorid_run.png)

Click Run  `Pixel 2 API 33`. Then you can click Run `app`.

You will see:

![](img/01_andorid_run.png)

## Running iOS app

You need mac to run iOS emulator.

You need to have XCode installed. 

Run `yarn cap open ios`. This will open XCode.

You can pick any device as emulator. I'm ok with iPod touch.

Then click 'Run'. XCode will build app and run it on the device.

![](img/01_xcode_run.png)

After build, you will see a running application on iOS emulator.

![](img/01_nuxt_ios.png)


## Congrats!

In this guide we turn our NuxtJS application into Android and iOS applications. 

