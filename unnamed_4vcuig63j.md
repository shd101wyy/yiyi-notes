---
note:
    id: ""
    tags: [frontend/pwa/splash-screen, frontend/react/create-react-app]
    modifiedAt: 2020-04-30T10:57:14.791Z
    createdAt: 2020-04-30T10:54:43.218Z
---
# PWA Splash screen
#frontend/pwa/splash-screen #frontend/react/create-react-app 

Check: https://dev.to/guillaumelarch/how-to-add-a-splash-screen-for-a-progressive-web-app-with-react-1019

And the following GitHub project

https://github.com/onderceylan/pwa-asset-generator

## How to add a splash screen for a progressive web app with React ?
guillaumelarch profile image guillaumel twitter logo Nov 5 '19 ・1 min read  

Recently I was developing a Progressive Web App (PWA). A PWA is a web app that you can install on your smartphone without downloading it from a Store, cool isn't it ? These kind of applications are not going to replace native web apps right now but it seems it has become an interesting technology.

Anyway, my problem was to add the splash screen of my application for iOS users. Now I know how to easily add a splash screen, and I will explain you how to reproduce the following exemple :

![](https://res.cloudinary.com/practicaldev/image/fetch/s--tYq43i4K--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/v57xrih2cs4dju1ev53c.gif)


First of all, we need to create a react app :

```
npx create-react-app my-app
```

Put your logo (we will call it logo.svg) in the public folder of the newly created react app. And now we are going to actually add the plash screen capability for all types of iPhone screen size :

```
cd my-app/public 
npx pwa-asset-generator logo.svg ./assets -i ./index.html -m ./manifest.json
```

The pwa-asset-generator script ([github repo](https://github.com/onderceylan/pwa-asset-generator)) generates all the existing iPhone screen sizes and puts it in the asset folder, but also it adds in the index.html and in the manifest.json all metadata needed by the phone to choose the correct image for the screen splash ! This tool is so amazing !

Now you can start the development server, take your 📱, access to your app with safari, install it to your phone and test the splash screen !
