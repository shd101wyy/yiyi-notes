---
tags:
  - frontend/react/create-react-app
created: 2020-05-01T12:36:59.886Z
modified: 2020-05-01T12:41:12.334Z
---

# Create a PWA Update Notification with Create React App

#frontend/react/create-react-app

[Source](https://felixgerschau.com/create-a-pwa-update-notification-with-create-react-app "Permalink to Create a PWA Update Notification with Create React App")

[![Update notification screenshot](https://felixgerschau.com/static/4ccaf02d115d2288a862d1b1650242ba/5a190/update-notification-screenshot.png)](https://felixgerschau.com/static/4ccaf02d115d2288a862d1b1650242ba/3fca6/update-notification-screenshot.png)

_If you're only interested in code just skip to "Implementation" further down._

When I first heard about progressive web apps (or PWAs) and their offline capabilities, the first question that came to my mind was how to gain control over which version the user is seeing.

What if an urgent bug-fix needs to be shipped as soon as possible to all users?

Here I'll first go over some basic concepts regarding progressive web apps and then I'll explain how to implement an "update available" notification for the user.

In single-page applications without server-side rendering, usually, the first thing being loaded is the application code, which then populates the app with data fetched from the API. This is also called [app shell architecture](https://developers.google.com/web/fundamentals/architecture/app-shell).

In other words, the "app shell" is everything that you would normally download from a mobile app store. In progressive web apps, we _cache_ the app shell with service workers.

Different [caching strategies](https://developers.google.com/web/tools/workbox/modules/workbox-strategies) can be selected when working with progressive web apps and service workers. The most widely used strategy is the [cache first](https://developers.google.com/web/tools/workbox/modules/workbox-strategies#cache_first_cache_falling_back_to_network) strategy, which first serves the application from the service worker cache and then updates the cache in the background to ensure fast and consistent user experience.

To get a better understanding of how this update works and at which point we can show the update notification to the user, it's important to have an overview of the lifecycle of a service worker:

## Service worker lifecycle

[![Service worker lifecycle](https://felixgerschau.com/static/eb3d02dbb5366f9923b5e42697329e38/5a190/sw-lifecycle.png)](https://felixgerschau.com/static/eb3d02dbb5366f9923b5e42697329e38/dcccd/sw-lifecycle.png)

When a service worker is being installed for the first time, it usually runs through all these states directly until it is **Activated**.

However, when a new version is getting installed, it will get stalled in the **Installed/waiting** state, waiting for getting activated.

Now we can get the service worker to become active in a few different ways:

**1\. Close all tabs and open the website again**

The service worker won't automatically pass on to _available_ unless **all** tabs with the website open have been closed.

**2\. Call _skipWaiting_ in the service-worker.js**

This simply tells the service worker to install and activate new versions as soon as they are available.

**3\. Call _skipWaiting_ on user interaction**

That's when the user clicks on the button of the banner we'll provide.

## How service workers are set up in Create-React-App

In _Create React App_ you might have noticed that it already comes with a `serviceWorker.ts` file (I'm using the TypeScript version here).

Behind the scenes, CRA is using [Workbox](https://developers.google.com/web/tools/workbox/), a tool by Google that handles all the set up with an easy to use API. CRA is using its GenerateSW _webpack plugin_. This plugin autogenerates a `service-worker.js` that already precaches static assets and registers the navigation route.

The `serviceWorker.ts` file then registers this service worker by calling

    navigator.serviceWorker.register(swUrl)

(swUrl points to the location of `service-worker.js`).

The part of the JS file that's most interesting to us is the following, which lets us tell the service worker to skip waiting and become active.

    self.addEventListener('message', (event) => {
      if (event.data && event.data.type === 'SKIP_WAITING') {
        self.skipWaiting();
      }
    });

## Debugging

Before continuing with the actual implementation it makes sense to talk about how to work with this while developing. Create React App does a good job at [explaining this](https://create-react-app.dev/docs/making-a-progressive-web-app/).

You need to run `npm run build` followed by `serve -s build`.

For triggering updates I found it easier to just modify the service worker file at `build/service-worker.js`. Adding a console.log statement already triggers an update since it changes the file hash.

## Implementation

The solution is pretty simple and can be divided into two steps:

1. Check if an update is available and show a message
2. Tell the service worker to skip waiting and reload the page

### 1\. Check if update is available

Luckily, the `register` function of the service worker also provides an `onSuccess` and `onUpdate` callback. `onUpdate` will only be called when a new service worker has been successfully installed.

We can leverage this callback for adding the code that triggers the update notification. Before that we need to remove the registration call from the `index.tsx` to not call it twice:

    const [showReload, setShowReload] = useState(false);
    const [waitingWorker, setWaitingWorker] = useState<ServiceWorker | null>(null);

    const onSWUpdate = (registration: ServiceWorkerRegistration) => {
      setShowReload(true);
      setWaitingWorker(registration.waiting);
    };

    useEffect(() => {
      serviceWorker.register({ onUpdate: onSWUpdate });
    }, []);

**Make sure this component is always rendered, otherwise, the service worker won't get registered!**

`registration.waiting` refers to the new worker. We save this service worker in the state of the component to to use it to skip waiting in the second step.

In case you were wondering: This example is using [React hooks](https://reactjs.org/docs/hooks-reference.html). Calling `useEffect` without a second parameter is like calling `componentDidMount` and `useState` replaces React's state. Check out their documentation for more information on that topic. A full example can be found at the bottom.

### 2\. Skip waiting and reload the page

At this point, we're already showing some kind of message but we're not able to update to the new version. The code for this is pretty straight forward:

    const reloadPage = () => {
      waitingWorker?.postMessage({ type: 'SKIP_WAITING' });
      setShowReload(false);
      window.location.reload(true);
    };

Here we're sending a message to the service worker to tell it to skip waiting and become active and then we do a hard reload of the page.

Putting it all together, we should have a component that looks something like the following.

Now you just need to include it somewhere in your application where it _always_ gets rendered and you're done.

<https://gist.github.com/fgerschau/ad265d2f678ca81bd520b88979c1b2ae>

    import React, { FC, useEffect } from 'react';
    import { Snackbar, Button } from '@material-ui/core';
    import * as serviceWorker from '../serviceWorker';

    const ServiceWorkerWrapper: FC = () => {
      const [showReload, setShowReload] = React.useState(false);
      const [waitingWorker, setWaitingWorker] = React.useState<ServiceWorker | null>(null);

      const onSWUpdate = (registration: ServiceWorkerRegistration) => {
        setShowReload(true);
        setWaitingWorker(registration.waiting);
      };

      useEffect(() => {
        serviceWorker.register({ onUpdate: onSWUpdate });
      }, []);

      const reloadPage = () => {
        waitingWorker?.postMessage({ type: 'SKIP_WAITING' });
        setShowReload(false);
        window.location.reload(true);
      };

      return (
        <Snackbar
          open={showReload}
          message="A new version is available!"
          onClick={reloadPage}
          anchorOrigin={{ vertical: 'top', horizontal: 'center' }}
          action={
            <Button
              color="inherit"
              size="small"
              onClick={reloadPage}
            >
              Reload
            </Button>
          }
        />
      );
    }

    export default ServiceWorkerWrapper;

## Subscribe to the Newsletter

Subscribe to get my latest content by email.
