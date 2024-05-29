---
layout: two-cols
---
# retryability
cypress:
```js {*|4}
cy.containst('01')
cy.containst('02')
cy.containst('03')
cy.containst('04')
```
playwright:
```js {*|4}
page.getByText('01')
page.getByText('02')
page.getByText('03')
page.getByText('04')
```

::right::
<video src="/images/stopwatch.mov" autoplay loop class="h-[80%] m-auto" />

<style>
.two-columns {
  gap: 2em;
}
</style>

<!-- 
- retryability is a really cool thing
- it will try to find an element and if it does not find it, it will run the command again
- [click] [click] in fact, if I just ran the last command, this test would still pass
- but as a result - and this is by design - test does not catch all the interactions
- it just captures the ones that it is interested in
- and it does one other thing
-->


---
layout: center
---

![](/images/arrow_1.png)

<!--
- let’s say this is a timeline of your test
-->

---
layout: center
---

![](/images/arrow_2.png)

<!--
- and these are your commands
-->

---
layout: center
---

![](/images/arrow_3.png)

<!--
- both pw and cy do something cool
- they do a snapshot of your DOM
- capture html, css at every command
- so that you can debug, when your test is flaky
-->

---
layout: center
---

![](/images/arrow_4.png)

<!--
- the only problem are these areas
- it’s like first three seconds of the stopwatch
- application is still doing stuff
- this is where flakiness happens
- everytime you have a flaky test is because something happened between your test commands
- and this becomes even bigger problem when your tests fail in CI
-->
