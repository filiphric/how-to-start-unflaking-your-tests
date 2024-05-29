---
layout: cover
---
Story #1:
# Metabase - missing spinner
<!-- 
- metabase is a business intelligence software
- what’s beautiful, they are open source
- recommend if you want to see how a large test suite looks like
- we helped them drive down their test flakes
- they have hundreds of e2e tests and a quite a bit of test flake
- with integration of Replay, we were able to drive it down to practically zero
- zero on their main branch, any new flakes introduced can be fixed quickly
-->

---
layout: default
---
# Metabase - missing spinner

### Test script:
```js
it('shows a spinner', () => {
  cy.visit('/dashboard')
  cy.findByTestId('loading-spinner')
    .should('be.visible')
  // ...rest of the test
})
```
<br>

### Error message:
```plain
❌ expected findByTestId(loading-spinner) to exist in the DOM
```

<!-- 
- fairly simple test
- wanted to make sure that users with slower connections know what is happening
- they will see an animation
- but that test failed over and over
- which was strange, because none of the code that it was testing has changed
- and this was frustrating for developers
-->

---
layout: default
---

# Metabase - missing spinner
<video src="/images/cmd_click.mov" autoplay loop />

<!--
- so we recorded their test with Replay browser and started debugging
- what you see is Replay DevTools
- on the left side as the video zooms in you can see number of executions
- this indicates how many times that line of code was executed (27 times)
- we can time travel to every execution of that line of code as you can see by the cmd click annotation
- and there - we could see that there was clearly a moment where that spinner element appeared
- so the problem - why the test was failing element appear too fast for test to catch it
-->

---
layout: default
---

# Metabase - missing spinner

```js {*|7|4}
var throttleFieldValuesRequest = (dashboard_id) => {
  const matcher2 = {
    method: "GET",
    url: `/api/dashboard/${dashboard_id}/params/${parameterDetails.id}/values`,
    middleware: true
  };
  cy.intercept(matcher2, (req) => req.on("response", (res) => res.setThrottle(10)));
};
```

<!--
- but Metabase team was smart enough to think about it
- they have implemented this throttling strategy
- [click]
- they would slow down the request by limiting kbps, which is very smart
- [click]
- but there is one catch - the response from this url
- because this test will resolve in different time if the response has 100kB vs. when it has 1MB
- so in the end, when response from this url was smaller, the test ran faster
- and spinner showed for shorter period of time
- in fact, it showed for such a short time, that Cypress was not able to catch it
-->

---
layout: default
---
# Metabase - missing spinner
````md magic-move
```js
var throttleFieldValuesRequest = (dashboard_id) => {
  const matcher2 = {
    method: "GET",
    url: `/api/dashboard/${dashboard_id}/params/${parameterDetails.id}/values`,
    middleware: true
  };
  cy.intercept(matcher2, (req) => req.on("response", (res) => res.setThrottle(10)));
};
```
```js
var throttleFieldValuesRequest = (dashboard_id) => {
  const matcher2 = {
    method: "GET",
    url: `/api/dashboard/${dashboard_id}/params/${parameterDetails.id}/values`,
    middleware: true
  };
  cy.intercept(matcher2, (req) => req.on("response", (res) => res.setDelay(100)));
};
```
````
<!-- 
- so in the end a simple solution was implemented to make the test stable
- instead of setThrottle
- [click] they set up setDelay
- and the test was fixed
-->

---
layout: center
---

# Recording CI:
cypress:
```
npx cypress run --browser replay-chromium
```
playwright:
```
npx playwright test --project replay-chromium
```

<!--
- this was largely thanks to the fact that we were able to time-travel to that line of code
- and that we were able to record their test on CI and bring those recordings to the Replay devtools
- the integration is simple - you just use the replay browser instead of chromium and then you will have your recordings available in an online dashboard
-->
