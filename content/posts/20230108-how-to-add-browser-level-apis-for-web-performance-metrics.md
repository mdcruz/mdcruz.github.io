---
author: "Marie Cruz"
title: "How to add browser-level APIs for web performance metrics"
date: "2023-01-08"
tags:
- testing
- performance-testing
- grafana-blog
- k6
- grafana
---

This post was originally published on the [Grafana](https://grafana.com/) blog which you can view [here](https://grafana.com/blog/2023/01/08/how-to-add-browser-level-apis-for-web-performance-metrics/).

This post explains how to get started with [k6 browser](https://github.com/grafana/xk6-browser), a [k6 experimental module](https://grafana.com/docs/k6/latest/javascript-api/#k6experimental) that adds browser-level APIs to interact with browsers and collect web performance metrics as part of your Grafana k6 tests.

You can watch the supplementary video below for a deep dive into browser testing with Grafana k6 or continue reading to understand what k6 browser is, how to write and run your first test, and take a look at the k6 browser API.

<iframe class="youtube-video" src="https://www.youtube.com/embed/N7VJ9X5yAKo?si=1_2DlEhtJY557IFe" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Testing beyond protocol level

Over the years, Grafana k6 has become known as the open source performance testing tool that provides the best developer experience. Most of our efforts have focused on providing a [load testing](https://grafana.com/load-testing/) tool that helps test your servers or backend systems. Our [comprehensive load testing guide](https://grafana.com/blog/2024/01/30/load-testing-websites/)—which outlines different [types of load testing](https://grafana.com/load-testing/types-of-load-testing/—recommends) keeping users in mind, and backend performance testing only addresses half of your performance testing efforts.

Suppose you test the user experience of your website and verify that there are no performance issues on a specific user journey. In that case, **you need to drive some of your performance testing efforts from a browser perspective and consider a more realistic end-to-end test of the user flow.**

Most [load testing tools](https://grafana.com/load-testing/load-testing-tools/) focus on testing API endpoints, but it’s different from what your users normally interact with. Your users interact with the browser, so it’s also vital to test the browser’s performance to get an end-to-end perspective of what’s happening when they’re interacting with your web applications.

Both frontend and backend performance testing has its pros and cons when done in isolation, which we discussed in more detail as part of the video below.

<iframe class="youtube-video" src="https://www.youtube.com/embed/xVACRP5qIJI?si=p7pj-ppcMWgEJ619" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

If you want to start expanding your performance testing use cases and test beyond the protocol level, this is where the k6 browser module comes in.

## What is k6 browser module?

The k6 browser module brings browser automation and end-to-end web testing to k6 while supporting core k6 features. It enables you to get insights from your frontend application during performance testing.

You can now mix browser-level and protocol-level tests in a single and unified script using k6. This can simulate the bulk of your traffic from your protocol-level tests and run one or two virtual users on a browser level to mimic how a user interacts with your website, thus leveraging a hybrid approach to performance testing.

The browser module offers a unique solution as you don’t have to use separate tools to test your frontend and backend systems. It also offers a simplified experience and aggregated view of performance metrics.

## Writing the test

Let’s simulate a user visiting a [test application](https://test.k6.io/) and logging in. You can also copy one of our example scripts to get started as part of our [k6 browser module documentation](https://grafana.com/docs/k6/latest/using-k6-browser/running-browser-tests/).

```
import { browser } from 'k6/experimental/browser';
import { check } from 'k6'

export const options = {
  scenarios: {
    ui: {
      executor: 'shared-iterations',
      options: {
        browser: {
          type: 'chromium',
        },
      },
    },
  }
}

export default async function () {
  const page = browser.newPage();

  try {
    await page.goto('https://test.k6.io/my_messages.php');

    page.locator('input[name="login"]').type('admin');
    page.locator('input[name="password"]').type('123');

    const submitButton = page.locator('input[type="submit"]');

    await Promise.all([
      page.waitForNavigation(),
      submitButton.click(),
    ]);

    check(page, {
      'header': page.locator('h2').textContent() == 'Welcome, admin!',
    });
  } finally {
    page.close();
  }
}
```

There are a lot of things happening in the preceding code, especially the introduction of [asynchronous operations](https://grafana.com/docs/k6/latest/using-k6-browser/running-browser-tests/#asynchronous-operations), so let’s break down what’s happening.

1. We are importing `browser` from the `k6/experimental/browser` module.
1. Next, we are using [options](https://grafana.com/docs/k6/latest/using-k6/k6-options/) to configure our test-run behavior. We also set the browser scenario option’s type to `chromium`, which is mandatory.
1. To create a new page in your browser instance, we use the `browser.newPage()` inside our [default function](https://grafana.com/docs/k6/latest/using-k6/test-lifecycle/#the-vu-stage), an existing k6 functionality.
1. We visit the page by using `page.goto` and pass the test application URL. `page.goto` is an asynchronous operation, so we need to wait for this to finish and use the `await` keyword.
1. Once the operation completes, we use [page.locator](https://grafana.com/docs/k6/latest/javascript-api/k6-browser/locator/) to interact with the elements we want. In the example, we are creating two locators. One for the login name and another one for the login password. We use the [type](https://grafana.com/docs/k6/latest/javascript-api/k6-browser/locator/type/) method to type the name and password into the fields.
1. To click the login button, we use the `click` method, which is an asynchronous operation. Clicking the submit button also causes page navigation which we need to wait to load, so `page.waitForNavigation()`, another asynchronous operation, is needed because the page won’t be ready until the navigation completes.
1. Since there are two asynchronous operations, we need to use `Promise.all([])` to wait for the two promises to be resolved before continuing to avoid any race conditions.
1. Next, we use the [check](https://grafana.com/docs/k6/latest/javascript-api/k6/check/) feature from k6 to assert the text content of a specific element.
1. Finally, we close the page and the browser.

## Running the test

To run the test, simply use the following command.

```
# K6_BROWSER_HEADLESS needs to be set to false
# if you want to see the browser launching
K6_BROWSER_HEADLESS=false k6 run script.js
```

If you face any issues running the command, please check out our [documentation for running the test](https://grafana.com/docs/k6/latest/using-k6-browser/running-browser-tests/#run-a-test).

With the browser launching, this provides a more visual experience as to what your users actually see so you can also find blind spots and catch issues related to browsers that won’t be detected on a protocol level.

## Browser metrics

When the test is finished running, additional [browser metrics](https://grafana.com/docs/k6/latest/using-k6-browser/metrics/) are tracked as part of the k6 summary output. HTTP requests that were triggered by the browser test are also reported.

```
  browser_data_received.......: 5.8 kB  2.6 kB/s
  browser_data_sent...........: 2.6 kB  1.2 kB/s
  browser_http_req_duration...: avg=121.97ms min=587µs    med=99.56ms  max=313.01ms p(90)=227.7ms  p(95)=270.35ms
  browser_http_req_failed.....: 0.00%   ✓ 0        ✗ 5
  browser_web_vital_cls.......: avg=0        min=0        med=0        max=0        p(90)=0        p(95)=0
  browser_web_vital_fcp.......: avg=309.94ms min=225.5ms  med=309.94ms max=394.39ms p(90)=377.5ms  p(95)=385.95ms
  browser_web_vital_fid.......: avg=200µs    min=200µs    med=200µs    max=200µs    p(90)=200µs    p(95)=200µs
  browser_web_vital_inp.......: avg=208ms    min=208ms    med=208ms    max=208ms    p(90)=208ms    p(95)=208ms
  browser_web_vital_lcp.......: avg=225.5ms  min=225.5ms  med=225.5ms  max=225.5ms  p(90)=225.5ms  p(95)=225.5ms
  browser_web_vital_ttfb......: avg=255.29ms min=198.09ms med=255.29ms max=312.5ms  p(90)=301.05ms p(95)=306.77ms
  checks......................: 100.00% ✓ 1        ✗ 0
  data_received...............: 0 B     0 B/s
  data_sent...................: 0 B     0 B/s
  iteration_duration..........: avg=1.43s    min=1.43s    med=1.43s    max=1.43s    p(90)=1.43s    p(95)=1.43s
  iterations..................: 1       0.449078/s
  vus.........................: 1       min=1      max=1
  vus_max.....................: 1       min=1      max=1
```

## k6 browser API

The [k6 browser API](https://grafana.com/docs/k6/latest/javascript-api/k6-browser/) aims to provide a rough compatibility with the [Playwright API](https://playwright.dev/docs/api/class-playwright) for NodeJS, meaning k6 users don’t have to learn an entirely new API.

At the moment, the k6 API is synchronous. However, since many browser operations happen asynchronously, and in order to follow the Playwright API more closely, we are working on migrating most of the browser methods to be asynchronous. This means that while k6 browser is ready to be used, be warned that our API is still undergoing a few changes.

At the moment, few methods such as `page.goto()`, `page.waitForNavigation()` and `Locator.click()` return [JavaScript promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises). Our goal is to support async and await syntax for all asynchronous operations for simplicity.

For more examples on how to use the k6 browser API, please check out [k6 browser examples](https://github.com/grafana/xk6-browser/tree/main/examples).

## A hybrid approach to performance testing

If you only consider web performance, this can lead to false confidence in your overall application performance when the amount of traffic to an application increases.

It’s still highly recommended to also test your backend systems to have a complete picture of your application’s performance, via the protocol level.

However, there are problems associated with testing via the protocol level, such as:

- Not being closer to the user experience since it’s skipping the browser.
- Scripts can be lengthy to create and difficult to maintain as the application grows.
- Browser performance metrics are ignored.

On the other hand, if you perform load testing by spinning up a lot of browsers, this requires significantly more load-generation resources, which can become quite costly.

To address the shortcomings of each approach, a recommended practice is to adopt a [hybrid approach to performance testing](https://grafana.com/blog/2024/01/30/load-testing-websites/), which is a combination of testing both the backend and frontend systems via protocol and browser level. With a hybrid approach, you spin up the majority of your load via the protocol level, then simultaneously have one or two browser-level virtual users, so you can also have a view of what’s happening on the front end.

<figure>
  <img src="../../images/hybrid-testing.png" alt="An illustration of k6 testing frontend system as well as backend systems for a hybrid approach to performance testing">
</figure>

The great thing with k6 browser module is that it can offer you this hybrid approach to performance testing. While you can do this with multiple tools, the beauty of using this module is that it’s built on top of k6, which means that you can have a protocol-level and a browser-level test in the same script.

Let’s see how that translates to code with a [load testing example](https://grafana.com/load-testing/load-testing-examples/).

## Writing the hybrid test

A common scenario that we recommend is to mix a smaller subset of browser-level tests with a larger protocol-level test. To run a browser-level and protocol-level test concurrently, you can use [scenarios](https://grafana.com/docs/k6/latest/using-k6/scenarios/).

```
import { browser } from 'k6/experimental/browser';
import { check } from 'k6';
import http from 'k6/http';

export const options = {
  scenarios: {
    browser: {
      executor: 'constant-vus',
      exec: 'browserTest',
      vus: 1,
      duration: '10s',
      options: {
        browser: {
          type: 'chromium',
        }
      }
    },
    news: {
      executor: 'constant-vus',
      exec: 'news',
      vus: 20,
      duration: '1m',
    },
  },
};

export async function browserTest() {
  const page = browser.newPage();

  try {
    await page.goto('https://test.k6.io/browser.php');

    page.locator('#checkbox1').check();

    check(page, {
      'checkbox is checked': page.locator('#checkbox-info-display').textContent() === 'Thanks for checking the box',
    });
  } finally {
    page.close();
  }
}

export function news() {
  const res = http.get('https://test.k6.io/news.php');

  check(res, {
    'status is 200': (r) => r.status === 200,
  });
}
```

Let’s break down the preceding code:

1. We are using [options](https://grafana.com/docs/k6/latest/using-k6/k6-options/) to configure our test run behavior. In this particular script, we are declaring two scenarios to configure specific workload, one for the browser-level test called `browser` and one for the protocol-level test called `news`.
1. Both the `browser` and `news` scenario are using the [constant-vu executor](https://grafana.com/docs/k6/latest/using-k6/scenarios/executors/constant-vus/) which introduces a constant number of virtual users to execute as many iterations as possible for a specified amount of time.
1. Next, there are two JavaScript functions declared, `browser()` and `news()`. These functions contain the code that will be executed by a virtual user. The `browser(`) function, represents our browser-level test and simply visits a test URL, clicks a checkbox and verifies if the checkbox has been ticked successfully while the `news()` function, which represents our protocol-level test, sends a GET request to a different URL and checks if the status code is returning 200.
1. Since we are using scenarios, the two functions are independent from each other and therefore, runs in parallel.

## Running the test

Using the same k6 run command as above, you should see a similar test output as below:

```
✓ status is 200
✓ checkbox is checked

browser_data_received..........: 49 kB   818 B/s
browser_data_sent..............: 4.2 kB  69 B/s
browser_http_req_duration......: avg=184.57ms min=82.74ms  med=180.91ms max=309.42ms p(90)=294ms    p(95)=302.21ms
browser_http_req_failed........: 50.00%  ✓ 6          ✗ 6
browser_web_vital_cls..........: avg=0.000029 min=0        med=0        max=0.000177 p(90)=0.000088 p(95)=0.000133
browser_web_vital_fcp..........: avg=404.64ms min=362.19ms med=397.84ms max=476.8ms  p(90)=450.4ms  p(95)=463.6ms
browser_web_vital_fid..........: avg=266.66µs min=200µs    med=299.99µs max=300µs    p(90)=300µs    p(95)=300µs
browser_web_vital_inp..........: avg=16ms     min=16ms     med=16ms     max=16ms     p(90)=16ms     p(95)=16ms
browser_web_vital_lcp..........: avg=404.64ms min=362.19ms med=397.84ms max=476.8ms  p(90)=450.4ms  p(95)=463.6ms
browser_web_vital_ttfb.........: avg=279.33ms min=260.9ms  med=271.44ms max=309.09ms p(90)=302.6ms  p(95)=305.85ms
checks.........................: 100.00% ✓ 12653      ✗ 0
data_received..................: 20 MB   334 kB/s
data_sent......................: 1.4 MB  23 kB/s
http_req_blocked...............: avg=2.09ms   min=0s       med=2µs      max=287.89ms p(90)=3µs      p(95)=3µs
http_req_connecting............: avg=1.01ms   min=0s       med=0s       max=197.66ms p(90)=0s       p(95)=0s
http_req_duration..............: avg=92.76ms  min=81.05ms  med=93.14ms  max=425.17ms p(90)=102.79ms p(95)=104.21ms
  { expected_response:true }...: avg=92.76ms  min=81.05ms  med=93.14ms  max=425.17ms p(90)=102.79ms p(95)=104.21ms
http_req_failed................: 0.00%   ✓ 0          ✗ 12647
http_req_receiving.............: avg=62.6µs   min=13µs     med=35µs     max=8.1ms    p(90)=58µs     p(95)=83µs
http_req_sending...............: avg=10.72µs  min=4µs      med=10µs     max=598µs    p(90)=13µs     p(95)=16µs
http_req_tls_handshaking.......: avg=1.05ms   min=0s       med=0s       max=119.52ms p(90)=0s       p(95)=0s
http_req_waiting...............: avg=92.68ms  min=81.01ms  med=93.08ms  max=425.13ms p(90)=102.71ms p(95)=104.11ms
http_reqs......................: 12647   210.435473/s
iteration_duration.............: avg=95.5ms   min=81.14ms  med=93.26ms  max=1.31s    p(90)=103.03ms p(95)=105.15ms
iterations.....................: 12653   210.535308/s
vus............................: 20      min=20       max=21
vus_max........................: 21      min=21       max=21


running (1m00.1s), 00/21 VUs, 12653 complete and 0 interrupted iterations
browser ✓ [======================================] 1 VUs   10s
news    ✓ [======================================] 20 VUs  1m0s
```

Since it’s all in one script, this allows for greater collaboration among teams and a unified view of the performance metrics from a browser-level and protocol-level perspective.

## Get involved with k6

While k6 browser started off as an [extension](https://grafana.com/docs/k6/latest/extensions/), as of [k6 version 0.43.0](https://github.com/grafana/k6/releases/tag/v0.43.0), it is now bundled in k6 as an experimental module, and usable without a separate binary or compilation step! We also have further plans to integrate k6 browser in k6 cloud as part of a private beta. We consider browser automation an important part of web application testing, and we have big goals for k6 browser. Our [roadmap](https://github.com/grafana/xk6-browser/blob/main/ROADMAP.md) details essential status updates and our short, mid, and long-term goals.

With that said, we need your help! Since k6 browser is still relatively new, we need help from the community to try out the tool and give us feedback.

Check our [GitHub project](https://github.com/grafana/xk6-browser), read our [documentation](https://grafana.com/docs/k6/latest/javascript-api/k6-browser/), and play with the tool. If you find any issues, please raise them on our GitHub project or check out our [community forum](https://community.grafana.com/c/grafana-k6/k6-browser/79) for additional support.

## More k6 browser resources

- [Video: Browser testing with k6](https://www.youtube.com/watch?v=N7VJ9X5yAKo)
- [A k6 guide for load testing websites](https://grafana.com/blog/2024/01/30/load-testing-websites/)
- [Video: Frontend vs. backend performance testing](https://www.youtube.com/watch?v=xVACRP5qIJI)
- [k6 browser documentation](https://grafana.com/docs/k6/latest/javascript-api/k6-experimental/browser/)