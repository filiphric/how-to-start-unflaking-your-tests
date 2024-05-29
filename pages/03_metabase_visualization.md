---
layout: cover
---
Story #2:
# Metabase - Visualization

<!--
- this one was test flake that inspired the name of this presentation
- blink and you’ll miss it
- in fact, I’m going to show you a screenshot from failing and passing test side by side
-->

---
layout: default
---
# Metabase - Visualization

::div{class="flex gap-8 justify-between"}
![](/images/before.png)

![](/images/after.png)
::

---
layout: default
---

# Metabase - Visualization
<video src="/images/shift.mov" autoplay loop class="h-[80%] m-auto" />

<!-- 
- can you see it now?
- I’m going to tell you straight away, that it is the font change that caused the test to fail
- it was a simpl css change, but what’s really interesting is how it actually affected the functional side of tests
-->

---
layout: default
---

# Metabase - Visualization

### Error message:
```plain {*|3|4}
AssertionError: Timed out retrying after 4000ms: 
expected '<span.rounded.flex.align-center.text-bold.cursor-pointer>' 
to have text 'Created At is Apr 1, 12:00 AM – Sep 1, 2022, 12:00 AM', 
but the text was 'Created At is May 1, 12:00 AM – Sep 1, 2022, 12:00 AM'
```
<!-- 
- this was the most confusing part about it
- there was nothing that pointed to a problem with CSS
- in fact, it looked like a functional error
- [click] because we expected the date range to Start on Apr 1
- [click] but it actually started on May 1
-->

---
layout: default
---

```js {*|9-13|17-20}
it("should apply filters to the series selecting area range", () => {
  cy.viewport(1280, 800);

  visitQuestionAdhoc({
    dataset_query: testQuery,
    display: "line",
  });

	// select date range drag & drop
  cy.get(".Visualization")
    .trigger("mousedown", 100, 200)
    .trigger("mousemove", 230, 200)
    .trigger("mouseup", 230, 200);

  cy.wait("@dataset");

  cy.findByTestId("filter-pill").should(
    "have.text",
    "Created At is Apr 1, 12:00 AM – Sep 1, 2022, 12:00 AM",
  );

  cy.get(".Visualization .dot").should("have.length", 6);
});
```

<!--
- to give you a better idea of what the test did, here’s the test code
- [click] the test tried to select a range
- [click] and then validate that the range was indeed selected
-->

---
layout: default
---

# Metabase - Visualization

![](/images/network.png){class="h-90 m-auto"}

<!--
- a good place to look when bugs like these happen is in network panel
- since we recorded tests, we were able to look into the network panel and confirm, that the range selected started on May 1st
- so our test was doing something we didn’t expect
- it didn’t behave the way we wanted
-->

---
layout: default
---

# Metabase - Visualization
<video src="/images/shift.mov" autoplay loop class="h-[80%] m-auto" />

<!-- 
- going back to the video comparison
- we can see why the test did not work the way we wanted
- the css change caused the whole layout to shift
- so the cypress commands that relied on precise coordinates on page were selecting something different after we used bolder fonts
-->
