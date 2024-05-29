---
layout: center
---
# False positive flake

<!-- 
- we like our tests green
- but there are cases where a green tests hides a real issue
-->

---
layout: default
---
# False positive flake

```js
beforeEach(() => {
  cy.visit('/');
  cy.contains('Loading').should('not.exist');
  cy.injectAxe();
});

it('Checks accessibility of a page', () => {
  cy.checkA11y();
});
```

<!-- 
- show code, then go to demo
-->

---
layout: center
---
# Let’s debug!

<!-- 
- demo links:
  - failed: https://app.replay.io/recording/cypresse2ea11ycyts--5927ab03-d263-4c91-a159-68ea4c3d1269
  - passing: https://app.replay.io/recording/cypresse2ea11ycyts--c6e8789f-96e0-40dc-b528-c3cd07d8132a
-->


---
layout: default
---
# False positive flake

```js {*|8}
beforeEach(() => {
  cy.visit('/');
  cy.contains('Loading').should('not.exist');
  cy.injectAxe();
});

it('Checks accessibility of a page', () => {
  cy.get('.modal').should('be.visible')
  cy.checkA11y();
});
```
<!-- 
- the problem now is, that we did not make sure that application is ready for accessibility check
- [click] we need to make sure by check the modal presence first
-->
