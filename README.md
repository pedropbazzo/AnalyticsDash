# Segment Angular Quickstart Guide
<div align="center">
  <img src="https://user-images.githubusercontent.com/16131737/52240871-07ea4380-2887-11e9-95ac-094d66833bed.png"/>
  <p><b><i>You can't fix what you can't measure</i></b></p>
</div>

Analytics helps you measure your users, product, and business. It unlocks insights into your app's funnel, core business metrics, and whether you have product-market fit.

## How to get started
1. **Collect analytics data** from your app(s).
    - The top 200 Segment companies collect data from 5+ source types (web, mobile, server, CRM, etc.).
2. **Send the data to analytics tools** (for example, Google Analytics, Amplitude, Mixpanel).
    - Over 250+ Segment companies send data to eight categories of destinations such as analytics tools, warehouses, email marketing and remarketing systems, session recording, and more.
3. **Explore your data** by creating metrics (for example, new signups, retention cohorts, and revenue generation).
    - The best Segment companies use retention cohorts to measure product market fit. Netflix has 70% paid retention after 12 months, 30% after 7 years.

[Segment](https://segment.com?utm_source=github&utm_medium=click&utm_campaign=protos_angular) collects analytics data and allows you to send it to more than 250 apps (such as Google Analytics, Mixpanel, Optimizely, Facebook Ads, Slack, Sentry) just by flipping a switch. You only need one Segment code snippet, and you can turn integrations on and off at will, with no additional code. [Sign up with Segment today](https://app.segment.com/signup?utm_source=github&utm_medium=click&utm_campaign=protos_angular).

### Why?
1. **Power all your analytics apps with the same data**. Instead of writing code to integrate all of your tools individually, send data to Segment, once.

2. **Install tracking for the last time**. We're the last integration you'll ever need to write. You only need to instrument Segment once. Reduce all of your tracking code and advertising tags into a single set of API calls.

3. **Send data from anywhere**. Send Segment data from any device, and we'll transform and send it on to any tool.

4. **Query your data in SQL**. Slice, dice, and analyze your data in detail with Segment SQL. We'll transform and load your customer behavioral data directly from your apps into Amazon Redshift, Google BigQuery, or Postgres. Save weeks of engineering time by not having to invent your own data warehouse and ETL pipeline.

    For example, you can capture data on any app:
    ```javascript
    analytics.track('Order Completed', { price: 99.84 })
    ```
    Then, query the resulting data in SQL:
    ```sql
    select * from app.order_completed
    order by price desc
    ```

### 🚀 Startup Program
<div align="center">
  <a href="https://segment.com/startups"><img src="https://user-images.githubusercontent.com/16131737/53128952-08d3d400-351b-11e9-9730-7da35adda781.png" /></a>
</div>
If you are part of a new startup  (&lt;$5M raised, &lt;2 years since founding), we just launched a new startup program for you. You can get a Segment Team plan  (up to <b>$25,000 value</b> in Segment credits) for free up to 2 years — <a href="https://segment.com/startups/">apply here</a>!

# 🏃💨 Quickstart
In this tutorial you'll add your write key to this Angular demo app to start sending data from the app to Segment, and from there to any of our destinations, using our [Analytics.js library](https://segment.com/docs/sources/website/analytics.js?utm_source=github&utm_medium=click&utm_campaign=protos_angular). Once your app is set up, you'll be able to turn on new destinations with the click of a button! Ready to try it for yourself? Scroll down to the <a href="#demo">demo section</a> and run the app!

Start sending data from any [source](https://segment.com/docs/guides/general/what-is-a-source?utm_source=github&utm_medium=click&utm_campaign=protos_angular) and see events live in the Segment **debugger**:

<div align="center">
  <img src="https://user-images.githubusercontent.com/16131737/52242244-fe62da80-288a-11e9-98a5-b3c7bd3c891d.gif"/>
</div>
<br/>

Once you have data being sent to Segment, forward this data to any of our 250+ [destinations](https://segment.com/docs/guides/general/what-is-a-destination?utm_source=github&utm_medium=click&utm_campaign=protos_angular):

<div align="center">
  <img src="https://user-images.githubusercontent.com/16131737/52242246-ff940780-288a-11e9-8ab3-e5968df6810a.gif"/>
</div>

# 🔌 Installing on Your App
How do you get this in your own Angular app? Follow the steps below.

## ✂️ Step 1: Copy the Snippet
To install Segment in your own app first [sign up](https://app.segment.com/signup?utm_source=github&utm_medium=click&utm_campaign=protos_angular) with Segment and locate your Segment project's **Write Key**.
Then, copy and paste the snippet below into the `head` tag of your site. Replace `YOUR_WRITE_KEY` in the snippet below with your Segment project's write key.

> **Tip!** You can find your write key in your Segment project setup guide or settings.

```html
<script type="text/javascript">
  !function(){var analytics=window.analytics=window.analytics||[];if(!analytics.initialize)if(analytics.invoked)window.console&&console.error&&console.error("Segment snippet included twice.");else{analytics.invoked=!0;analytics.methods=["trackSubmit","trackClick","trackLink","trackForm","pageview","identify","reset","group","track","ready","alias","debug","page","once","off","on"];analytics.factory=function(t){return function(){var e=Array.prototype.slice.call(arguments);e.unshift(t);analytics.push(e);return analytics}};for(var t=0;t<analytics.methods.length;t++){var e=analytics.methods[t];analytics[e]=analytics.factory(e)}analytics.load=function(t,e){var n=document.createElement("script");n.type="text/javascript";n.async=!0;n.src="https://cdn.segment.com/analytics.js/v1/"+t+"/analytics.min.js";var a=document.getElementsByTagName("script")[0];a.parentNode.insertBefore(n,a);analytics._loadOptions=e};analytics.SNIPPET_VERSION="4.1.0";
  analytics.load("YOUR_WRITE_KEY");
  // analytics.page() // Uncomment if your application is NOT an SPA
  }}();
</script>
```

Typescript cannot identify the `analytics` property on the `window` object. To use `window.analytics` in your app, you need to **re-declare** the global variable that extends `window`.
In our example app, we put the following declaration in `app.module.ts`:
```javascript
declare global {
  interface Window { analytics: any; }
}
```

Now `window.analytics` is loaded and available to use throughout your app!

In the next sections you'll build out your implementation to track page loads, to identify individual users of your app, and track the actions they take.

## 📱 Step 2: Track Page Views in an SPA
> **Tip!** If your Angular application is **not** a Single Page application, you can uncomment the section in the above snippet and skip to Step 3.

The snippet from Step 1 loads `Analytics.js` into your app and is ready to track page loads. However, most Angular apps are a Single Page App (SPA), and in SPAs clicking a link or a new tab does not reload the whole webpage.

The `page` method lets you record page views on your website, along with optional information about the page being viewed. You can read more about how this works in the [page reference](https://segment.com/docs/sources/website/analytics.js/#page?utm_source=github&utm_medium=click&utm_campaign=protos_angular).

This means that using `analytics.page()` in `index.html` on a SPA will not detect page component loads, and you'll need to simulate a page load some other way. You can use Angular's in-house [routing](https://angular.io/guide/router) and lifecycle hooks to create `page` calls.

If you separate your pages into their own components and allow the [`RouterOutlet`](https://angular.io/guide/router#router-outlet) component to handle when the page renders, you can use `ngOnInit` to invoke `page` calls. The example below shows one way you could do this.

```javascript
@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {
  ngOnInit() {
    window.analytics.page('Home');
  }
}
```

## 🔍 Step 3: Identify Users
The `identify` method is how you tell Segment who the current user is. It includes a unique User ID and any optional traits you can pass on about them. You can read more about this in the [identify reference](https://segment.com/docs/sources/website/analytics.js/#identify?utm_source=github&utm_medium=click&utm_campaign=protos_angular).

**Note:** You don't need to call `identify` for anonymous visitors to your site. Segment automatically assigns them an `anonymousId`, so just calling `page` and `track` still works just fine without `identify`.

Here's what a basic call to `identify` might look like:

```javascript
window.analytics.identify('f4ca124298', {
  name: 'Michael Bolton',
  email: 'mbolton@initech.com'
});
```

This call identifies Michael by his unique User ID and labels him with `name` and `email` traits.

In Angular, if you have a form where users sign up or log in, you can use the `(ngSubmit)` handler to call `identify`, as in the example below:

```javascript
@Component({
  selector: 'app-identify-form',
  template: `
    <form #f="ngForm" (ngSubmit)="onSubmit(f)">
      <input name="name" type="text" ngModel required>
      <input name="email" type="email" ngModel required>
      <input type="submit" value="Submit">
    </form>
  `
})
export class IdentifyFormComponent {
  onSubmit(form: NgForm) {
    // Add your own unique ID here or we will automatically assign an anonymousID
    window.analytics.identify({
      name: form.value.name,
      email: form.value.email
    });
  }
}
```

> **Tip!** Other handlers might be better for other situations. You can see the [Angular docs on event handlers](https://angular.io/guide/user-input) for more information.

## ⏰ Step 4: Track Actions
The `track` method is how you tell Segment about which actions your users are performing on your site. Every action triggers what we call an "event", which can also have associated properties. It is important to figure out exactly what events you want to `track` instead of tracking anything and everything. A good way to do this is to build a [tracking plan](https://segment.com/docs/guides/sources/can-i-see-an-example-of-a-tracking-plan?utm_source=github&utm_medium=click&utm_campaign=protos_angular). You can read more about `track` in the [track reference](https://segment.com/docs/sources/website/analytics.js/#track?utm_source=github&utm_medium=click&utm_campaign=protos_angular).

Here's what a call to `track` might look like when a user bookmarks an article:

```javascript
window.analytics.track('Article Bookmarked', {
  title: 'Snow Fall',
  subtitle: 'The Avalanche at Tunnel Creek',
  author: 'John Branch'
});
```

The snippet tells us that the user just triggered the **Article Bookmarked** event, and the article they bookmarked was the `Snow Fall` article authored by `John Branch`. Properties can be anything you want to associate to an event when it is tracked.

### Track Calls with Event Handlers
In Angular, you can use several event handlers, such as `(click)`, `(submit)`, `(mouseenter)`, to call the `track` events. In the example below, we use the `(click)` handler to make a `track` call to log a `User Signup`.

```javascript
@Component({
  selector: 'app-signup-btn',
  template: `
    <button (click)="trackEvent()">
      Signup with Segment today!
    </button>
  `
})
export class SignupButtonComponent {
  trackEvent() {
    window.analytics.track('User Signup');
  }
}
```

> **Tip!** Other handlers might be better for other situations. You can see the [Angular docs on event handlers](https://angular.io/guide/user-input) for more information.

### Track Calls with Lifecycle Hooks
[Lifecycle hooks](https://angular.io/guide/user-input) are also great for tracking particular events, and in fact we used a lifecycle hook in Step 2 to track page component loads. For example, if you want to track components that are conditionally rendered from a parent component or a [`*ngIf`](https://angular.io/api/common/NgIf) conditional, then you can use `ngOnInit` to trigger a `track` event:

```javascript
@Component({
  selector: 'app-video-player',
  template: `
    <video autoplay>
      <source src="https://www.youtube.com/watch?v=dQw4w9WgXcQ" type="video/youtube">
    </video>
  `
})
export class VideoPlayerComponent implements OnInit {
  ngOnInit() {
    window.analytics.track('Video Played');
  }
}
```

> **Tip!** Other hooks might be better for other situations. You can see the [Angular docs on lifecycle hooks](https://angular.io/guide/lifecycle-hooks) for more information.

### Track Calls with Transitions
[Transition components](https://angular.io/guide/transition-and-triggers) control when UI elements render. Transitions, such as `@animation.start` and `@animation.done`, are fired at different times during a component lifecycle. In this example, when the `Toggle` button is clicked, some text is rendered, and the `@animation.done` trigger fires a `track` event.

```javascript
@Component({
  selector: 'app-panel',
  animations: [
    trigger(
      'animation', [
        transition(':enter', [
          style({ transform: 'translateX(100%)', opacity: 0 }),
          animate('500ms', style({ transform: 'translateX(0)', opacity: 1 }))
        ]),
        transition(':leave', [
          style({ transform: 'translateX(0)', opacity: 1 }),
          animate('500ms', style({ transform: 'translateX(100%)', opacity: 0 }))
        ])
      ]
    )
  ],
  template: `
    <button (click)="show = !show">Toggle</button>
    <div *ngIf="show" (@animation.done)="onAnimationEvent($event)">
      Integrate with over 200+ destinations!
    </div>
  `
})
export class PanelComponent {
  show: boolean = false;

  onAnimationEvent(event: AnimationEvent) {
    window.analytics.track('Destinations Info Toggled');
  }
}
```

## 🤔 What's next?
Once you've added a few track calls, **you're done**! You've successfully installed `Analytics.js` tracking. Now you're ready to see your data in the Segment dashboard, and turn on any destination tools. 🎉

You may wondering what you can be doing with all the raw data you are sending to Segment from your Angular app. With our [warehouses product](https://segment.com/product/warehouses?utm_source=github&utm_medium=click&utm_campaign=protos_angular), your analysts and data engineers can shift focus from data normalization and pipeline maintenance to providing insights for business teams. Having the ability to query data directly in SQL and layer on visualization tools can take your product to the next level.

## 💾 Warehouses
A warehouse is a special subset of destinations where we load data in bulk at a regular intervals, inserting and updating events and objects while automatically adjusting their schema to fit the data you've sent to Segment. We do the heavy lifting of capturing, schematizing, and loading your user data into your data warehouse of choice.

Examples of data warehouses include Amazon Redshift, Google BigQuery, MySQL, and Postgres.

<div align="center">
  <img src="https://user-images.githubusercontent.com/16131737/52260773-fed79180-28db-11e9-9bc2-f48161f155d5.gif"/>
</div>

## 📺 <span name="demo">Demo</span>
To start with this demo app, follow the instructions below:

1. [Sign up](https://app.segment.com/signup?utm_source=github&utm_medium=click&utm_campaign=protos_angular) with Segment and edit the snippet in [index.html](https://github.com/segmentio/analytics-angular/blob/master/src/index.html#L11) to replace `YOUR_WRITE_KEY` with your Segment **Write Key**.
    > **Tip!** You can find your key in your project setup guide or settings in the Segment.

    Your snippet will look something like the example below.

    ```html
    <script type="text/javascript">
      !function(){var analytics=window.analytics=window.analytics||[];if(!analytics.initialize)if(analytics.invoked)window.console&&console.error&&console.error("Segment snippet included twice.");else{analytics.invoked=!0;analytics.methods=["trackSubmit","trackClick","trackLink","trackForm","pageview","identify","reset","group","track","ready","alias","debug","page","once","off","on"];analytics.factory=function(t){return function(){var e=Array.prototype.slice.call(arguments);e.unshift(t);analytics.push(e);return analytics}};for(var t=0;t<analytics.methods.length;t++){var e=analytics.methods[t];analytics[e]=analytics.factory(e)}analytics.load=function(t,e){var n=document.createElement("script");n.type="text/javascript";n.async=!0;n.src="https://cdn.segment.com/analytics.js/v1/"+t+"/analytics.min.js";var a=document.getElementsByTagName("script")[0];a.parentNode.insertBefore(n,a);analytics._loadOptions=e};analytics.SNIPPET_VERSION="4.1.0";
      analytics.load("YOUR_WRITE_KEY");
      }}();
    </script>
    ```

2. From the command line, use `npm install` to install the dependencies, then `npm start` to run the app.
    ```bash
    npm install
    npm start
    ```

3. Go to the Segment site, and in the Debugger look at the live events being triggered in your app. You should see the following:
    - Page event: `Home` - When someone views the `home` page.
    - Page event: `About` - When someone views the `about` page.
    - Track event: `Learn Angular Link Clicked` - When someone clicks the "Learn Angular" link.

Congrats! You're seeing live data from your demo Angular app in Segment! 🎉

## 📝 Docs & Feedback
Check out our full [Analytics.js reference](https://segment.com/docs/sources/website/analytics.js?utm_source=github&utm_medium=click&utm_campaign=protos_angular) to see what else is possible, or read about the [Tracking API methods](https://segment.com/docs/sources/server/http?utm_source=github&utm_medium=click&utm_campaign=protos_angular) to get a sense for the bigger picture. If you have any questions, or see anywhere we can improve our documentation, [let us know](https://segment.com/contact?utm_source=github&utm_medium=click&utm_campaign=protos_angular)!
