---
layout: default
---

# Testing pyramid
<img src="/images/pyramid.png" class="h-70 m-auto mt-10" />

<!--
- at the risk of half people leaving my presentation, I’m going to show the testing pyramid
- probably all know this model 
- e2e tests costly and slow, unit tests are cheap and fast
- i believe that this model is very outdated
- we have much better tools nowadays
- especially the cost of creating a test is not so high
- as a result we now have more of them
-->

---
layout: default
---

# Testing speed
<img src="/images/turtle.png" class="h-10 m-auto mt-[16%]" />

<!--
- but let’s stop at this little guy
- because as I said, we have some great tools nowadays
- but they all seem to be battling for speed of execution
- I have read countless blog posts comparing pw and cy or some other tools
- they are focusing on execution
-->

---
layout: section
---

# Speed of execution vs. speed of maintenance

![](/images/speed.png){class="h-70 m-auto mt-10"}

<!--
- but if you answered "yes" to any of the questions from before, you know there’s a difference between speed of execution and speed of maintenance
- the funny thing is that we sometimes don’t notice this
- when it takes 30 minutes to run your tests in the pipeline, we as developers are happy to spend a month of our time migrating from one testing tool to another so that we can take it down to 25 minutes
- and we do all this work, because we believe that maintaining e2e tests is just too much work - still mentally living at the top of the pyramid
- and to be completely honest, I think in many way we really are living at the top of that pyramid, example
-->

---
layout: default
---

# Flaky test "fixing" strategies
![](/images/strategies.png){class="h-80"}

<!--
- we have these so-called strategies helping us live with the fact that our tests are flaky
- I’ve seen these strategies across many companies I’ve worked with
- muting a test
- quarantining a test
- retrying a test
- adding a longer timeout
- reproducing a test locally
- deleting a test
-->

---
layout: default
---
# Flaky test "fixing" strategies

![](/images/stages_of_grief.png){class="h-80"}

<!-- 
- or as I like to call them:
- shock
- denial 
- anger
- bargaining 
- depression
- acceptance
-->

---
layout: center
---
# Where do test flakes come from?

<!-- 
- first of all, definition
- a test that gives failing and passing result across multiple runs without any known changes in test code or app code
- and some people will say, that the biggest contributor to test flakiness is the environment on which the test is ran
- but I actually don’t agree
- I think different environments only reveal the issues that were already there
- and I have an argument to back it up
-->

---
layout: center
---
Some issues appear
when the environment <span v-mark="{type: 'box', color: '#F02D5E', at: 0}">is slow</span>

Some issues appear 
when the environment <span v-mark="{type: 'box', color: '#41B0F6', at: 0}">is fast</span>

<style>
  p {
    font-size: 2em;
    margin: 50px 0;
  }
</style>

<!--
- some issues appear because the environment is slow 
- some issues appear because the environment is fast
- another thing I keep hearing:
-->

---
layout: center
---
# Our testing tools are <span v-mark="{type: 'crossed-off', color: '#F02D5E', at: 1, strokeWidth: 4}">flaky</span>!

<!-- 
- no they are not [click]
- everybody migrating from PW -> Cypress
- I have some bad news, it’s not going to fix your tests
- don’t get me wrong, PW is great, you should use it, but it’s not going to solve flakiness
-->

---
layout: center
---

![](/images/cory.png){class="w-140"}

<!--
- I actually believe quite strongly, that although we often talk about flaky tests, it’s actually the application that’s flaky
- as mentioned in this tweet
- but tools can help - actually a lot
- one of the ways to tackle flaky apps is retryability
-->

---
src: ./01b_retryability.md
---

---
layout: center
---

<div v-click>
  <div class="absolute left-480px top-220px color-[#F02D5E]">
  CI
  </div>
  <div class="absolute left-730px top-220px color-[#F02D5E]">
  CI
  </div>
</div>

<div>
What happens in <span v-mark="{type: 'strike-through', color: '#F02D5E', at: 1, strokeWidth: 5}">Vegas</span>, stays in <span v-mark="{type: 'strike-through', color: '#F02D5E', at: 1, strokeWidth: 5}">Vegas</span>
</div>

<style>
  * {
    font-size: 2rem
  }
</style>

<!--
- [click] because we have limited visibility into what is happening in CI
- many times, we are simply left with an error message:
-->

---
layout: two-cols
---
# e2e flakiness

```plain
❌ Timed out retrying: Expected to find element, but never found it.
```

```plain
❌ failed because the element cannot be interacted with
```

```plain
❌ failed because the element is covered by another element
```

```plain
❌ failed because the page updated
```

```plain
❌ ...
```

<!-- 
- The main problem when dealing with e2e tests is the lack of context
- e2e test usually copies a user story
- when element is not found, it tells us nothing about what exactly failed
- when a test ends up with an error like this, the problem that caused this error happened a couple of steps before
- I want to share a couple of stories with you that start with a mysterious message like this
- these are stories I either encountered personally, or with our clients at Replay
- one of those clients is Metabase
 -->
