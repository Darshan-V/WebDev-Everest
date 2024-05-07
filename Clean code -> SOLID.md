### What is clean code ?
	Clean code is the term used to describe readable, maintainable and understandable code. Clean code is written in such a way that it is simple and expressive


#### Example 

```javascript
function calculateOrderTotal(order) {
  let total = 0;
  for (const item of order.items) {
    total += item.price * item.quantity;
  }

  if (order.promoCode) {
    total -= total * 0.1;
  }

  if (total > 1000) {
    total -= 100;
  }

  return total;
}
```
- In the above javascript function a single function in responsible to give a result which is making the code hard to read, 
- the above code can be split into multiple functions which does simple and single task, like *calculatingSubTotal*, *applyPromoCode*, *applyDiscountIfApplicable*, like the example in the below typescript code


```typescript
interface Order {
  items: Array<{ price: number; quantity: number }>;
  promoCode?: string;
}

function calculateOrderTotal(order: Order): number {
  const subtotal = calculateSubtotal(order.items);
  const discountedTotal = applyPromoCode(subtotal, order.promoCode);
  return applyDiscountIfApplicable(discountedTotal);
}

function calculateSubtotal(items: Array<{ price: number; quantity: number }>): number {
  return items.reduce((acc, item) => acc + item.price * item.quantity, 0);
}

function applyPromoCode(subtotal: number, promoCode?: string): number {
  if (promoCode) {
    return subtotal - subtotal * 0.1;
  }
  return subtotal;
}

function applyDiscountIfApplicable(total: number): number {
  if (total > 1000) {
    return total - 100;
  }
  return total;
}
```

- Using comments to say the intent of the code 
- Using whitespaces 
- Avoid using nested conditionals
- Instead of having global variables, encapsulate with typescript module
- using const and let instead of var
- using template literals for string interpolation
- using arrow functions, arrow functions provide more concise and readable syntax compared to traditional function expressions, they automatically bind to the surrounding context, which can reduce code verbosity
- using object destructuring
- using clear **verb-noun** combination for function names to convey it's purpose

### Following SOLID principles leads to the *clean code*
[SOLID](https://dev.to/carlosazaustre/solid-principles-in-javascript-123c)

**S-> Single Responsibility** : Each class or function should have a single responsibility.
**O -> Open-Closed** : Software entities (modules, classes, functions, etc) should be open for extension, but closed for modification.
**L -> Liskov Substitution Principle** : objects of a superclass should be replaceable by objects of a subclass without affecting the correctness of the program
```javascript
// example function to make HTTP requests
function makeRequest(url, errorHandler) {
    fetch(url)
        .then(response => response.json())
        .catch(error => errorHandler.handle(error))
    }

// We can have several functions to handle errors
const consoleErrorHandler = function handle(error){
    console.log(error)
}

const externalErrorHandler = function handle(error){
    sendErrorToExternalService(error)
}

makeRequest(url, consoleErrorHandler);
makeRequest(url, externalErrorHandler);
```
Example provided with a `makeRequest()` function and how different error handler objects can be used interchangeably without causing errors in the program.
**I -> interface segregation** : The **Interface Segregation Principle** states that a client should never be forced to implement an interface that it does not use, or clients shouldn't be forced to depend on methods they do not use
```javascript
class User {
  constructor(username) {
    this.username = username;
  }

  skipAd() {
    console.log(`I'm going to skip if I'm premium`);
  }

  startParty() {
    console.log(`I'm going to start a party if I'm premium`);
  }
}

class FreeUser extends User {
  constructor(username) {
    super(username);
  }

  skipAd() {
    return null;
  }

  startParty() {
    return null;
  }
}

class PremiumUser extends User {
  constructor(username) {
    super(username);
  }

  skipAd() {
    console.log(`Ad was skipped.`);
  }

  startParty() {
    console.log(`Party started, invite your friends!`);
  }
}

// The following code will execute
// and print the message
const premium = new PremiumUser(`premium_username`);
premium.skipAd();
premium.startParty();

// The following code will execute
// but return null
const free = new FreeUser(`free_username`);
free.skipAd();
free.startParty();
```
Taking into consideration that only a `PremiumUser` is able to skip ads and start parties, we are violating the **Interface Segregation Prinicple** by forcing both `User` and `FreeUser` to implement these actions
```javascript 
class User {
  constructor(username) {
    this.username = username;
  }
}

class FreeUser extends User {
  constructor(username) {
    super(username);
  }
}

class PremiumUser extends User {
  constructor(username) {
    super(username);
  }
}

const premiumBenefits = {
  skipAd() {
    console.log('Ad was skipped.');
  },
  startParty() {
    console.log('Party started, invite your friends!');
  },
};

Object.assign(PremiumUser.prototype, premiumBenefits);

// The following code will execute
// and print the message
const premium = new PremiumUser('premium_username');
premium.skipAd();
premium.startParty();

// The following code will throw an exception
// because a FreeUser does not implement these methods
const free = new FreeUser('free_username');
free.skipAd();
free.startParty();
```
