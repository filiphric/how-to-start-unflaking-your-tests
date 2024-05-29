---
layout: cover
---
Story #3:
# Race condition test flake

<!-- 
- our next story is very common 
- if you do testing chances are you have already seen an issue like this
-->


---
layout: two-cols
---
```js {*|3}
it('adds a product to cart', () => {
  cy.visit('/')
  cy.wait(1000)
  cy.contains('Add to Cart').should('be.visible').click()
  cy.contains('Product added to cart!').should('be.visible')
})
```

::right::
<img src="/images/shoe_failed.png" class="relative top-[10%]" />

<style>
.two-columns {
  gap: 1rem;
  grid-template-columns: 3fr 5fr !important;
}

.slidev-code-wrapper {
  padding-top: 35%
}
</style>

<!--
- example with a sneaker ecommerce site
- Cypress test that attempts to buy a sneaker
- flaky - fails and passes
- when fails - we see we click the button too soon
- [click] fixing on the test - add waiting so the test does not go that fast (`cy.wait(1000)`)
- but this is a slippery slope, because we may be increasing that time, not fixing the real problem
-->

---
layout: two-cols
---
# What do we know
<v-clicks>

- test opens page and clicks on "Add to cart" button
- test sometimes passes and sometimes fails
- error says: could not find the text: "Product added to cart!"
- counter shows number 0
- test seems to click the "Add to cart" button too soon
- test runs ok locally but fails on CI
</v-clicks>

::right::
<img src="/images/shoe_failed.png" class="relative top-[20%]" />

<style>
.two-columns {
  gap: 1rem;
  grid-template-columns: 5fr 5fr !important;
}

</style>

<!-- 
- so let’s debug this together
- we are going to dive into the code and figure out what actually made the test flaky
-->

---
layout: center
---
# Let’s debug!

<!-- 
https://app.replay.io/recording/4f19c5cc-50e2-490c-832b-9fa4d1cd64dc
-->

---
layout: default
---

parent component:
````md magic-move
```tsx
<Counter quantity={quantity} setQuantity={setQuantity} />
<AddToCartButton onClick={addToCart} />
```
```tsx
<Counter quantity={quantity} setQuantity={setQuantity} />
<AddToCartButton onClick={addToCart} isDisabled={quantity === 0} />
```
````

button component:
````md magic-move
```tsx
interface AddToCartButtonProps {
  onClick: () => void;
}

const AddToCartButton = ({ onClick }: AddToCartButtonProps) => (
  <button 
    className='bg-pink-500 text-white font-semibold text-2xl px-4 py-2 rounded-sm my-4'
    onClick={onClick} >
    Add to Cart
  </button>
);

export default AddToCartButton;
```
```tsx
interface AddToCartButtonProps {
  onClick: () => void;
  isDisabled: boolean;
}

const AddToCartButton = ({ onClick, isDisabled }: AddToCartButtonProps) => (
  <button 
    className='bg-pink-500 text-white font-semibold text-2xl px-4 py-2 rounded-sm my-4'
    onClick={onClick} >
    Add to Cart
  </button>
);

export default AddToCartButton;
```
```tsx
interface AddToCartButtonProps {
  onClick: () => void;
  isDisabled: boolean;
}

const AddToCartButton = ({ onClick, isDisabled }: AddToCartButtonProps) => (
  <button 
    className='bg-pink-500 text-white font-semibold text-2xl px-4 py-2 rounded-sm my-4'
    onClick={onClick}
    disabled={isDisabled} >
    Add to Cart
  </button>
);

export default AddToCartButton;
```
```tsx
interface AddToCartButtonProps {
  onClick: () => void;
  isDisabled: boolean;
}

const AddToCartButton = ({ onClick, isDisabled }: AddToCartButtonProps) => (
  <button 
    className='bg-pink-500 text-white font-semibold text-2xl px-4 py-2 rounded-sm my-4 disabled:opacity-20'
    onClick={onClick}
    disabled={isDisabled} >
    Add to Cart
  </button>
);

export default AddToCartButton;
```
````


<!--
- let’s add a new property
- [click] when the number is anything else than 0, isDisabled will be false
- [click] add it to our button as well
- [click] we’ll add an html attribute
- [click] and then add some styling so it’s semi transparent
-->

---
layout: default
---

````md magic-move
```js
it('adds a product to cart', () => {
  cy.visit('/');
  cy.wait(1000);
  cy.contains('Add to Cart').should('be.visible').click();
  cy.contains('Product added to cart!').should('be.visible');
});
```
```js
it('adds a product to cart', () => {
  cy.visit('/');
  cy.contains('Add to Cart').should('be.visible').click();
  cy.contains('Product added to cart!').should('be.visible');
});
```
````

<video src="/images/flake_fix.mov" autoplay loop class="w-120 mt-8 mx-auto" />

<!--
- as a result, our test code becomes cleaner
- [click]
- it accomplishes two things:
  - fixes hidden issues that customers actually have but never report
  - makes test automation more stable and therefore more reliable
- I am very confident that more than 90% of what we call flaky test is actually a flaky app, it has nothing to do with the test code itself
- this is FINE
-->
