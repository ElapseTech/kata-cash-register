# Cash Register Kata

Let's build a cash register! Looks simple? Then let's add some constraints...

## The rules

### Step 1

As you would normally implement it, code the feature `Producing a receipt` and the first scenario of the feature `Pricing`.

_See [here](#the-features) for the list of features_

### Step 2

Reimplement [Step 1](#step-1), but this time with the following constraints:
* No getter/setter allowed
* No return values nor _out_ parameters

### Step 3

Add scenarios 2 and 3 of the feature `Pricing` and add the following constraint:
* Only one level of indentation

### Step 4

Refactor your code to comply with the following constraint:
* Wrap all primitives and strings

### Step 5

Add the remaining scenarios and add the following constraint:
* No `if/else` nor `switch/case` in _production_ code

For example, this is not a valid implementation (nor a good design):

```java
class Product {
  public Double calculatePriceWithRebate() {
    if (DiscountType.QUANTITY.equals(this.discountType) && this.quantity >= this.minQuantity) {
      return (this.unitPrice - this.quantityRebate) * this.quantity;
    } else if (DiscountType.WEIGHT.equals(this.discountType) && this.weight >= this.minWeight) {
      return (this.unitPrice - this.weightRebate) * this.weight;
    }
  }
}
```

### Step 6

Have fun picking additional constraints from the [Object Calisthenics](https://williamdurand.fr/2013/06/03/object-calisthenics/)

_Note: Objects calisthenics are meant to put into **practice** some of the best practices of Object-Oriented Programming. They are **not** meant to be rules to follow in production code._

## The alternatives

### Alternative 1

Write all steps above in TDD.

### Alternative 2

Write all steps above in [ping-pong pair-programming](https://martinfowler.com/articles/on-pair-programming.html#PingPong)

### Alternative 3

Write all steps above in [mob programming](https://github.com/willemlarsen/mobprogrammingrpg)

### Alternative 4

Write all steps above in [TCR](https://medium.com/@kentbeck_7670/test-commit-revert-870bbd756864)

### Alternative 5

For all the steps above, follow this procedure:

0. Think of a behavior you have to implement
1. Start with writing the name of a test to verify that behavior
2. Start a **2min** timer (yes, 2 minutes 😉)
3. Implement both the test and the code
4. Once the timer ends
    - if the test pass: commit and pick the next behavior
    - if the test fail: revert your changes and start over
5. Repeat until a scenario is completed

## The features

### Producing a receipt

```gherkin
Feature: Producing a receipt

  Scenario: The one where there are no products
    Given no products scanned
    When produce receipt
    Then the subtotal is 0.00$
    And the total is 0.00$

  Scenario: The one where there are some products
    Given the following products are scanned:
    | sku | description | unit price |
    | abc | milk        | 1.25$      |
    | def | biscuits    | 4.95$      |
    When produce receipt
    Then the subtotal is 6.20$
    And the total is present
```

### Pricing

```gherkin
Feature: Pricing

  Scenario: The one where products are taxable
    Given there are the following tax rates:
    | code | rate  |
    | TPS  | 5%    |
    | TVQ  | 9.75% |
    And the following products are scanned:
    | sku | description | unit price | taxable |
    | ghi | mushrooms   | 1.25$      | no      |
    | jkl | soda        | 4.95$      | yes     |
    When produce receipt
    Then the total is 6.94$

  Scenario: The one with a quantity discount
    Given there is the following discount:
    | code           | discount | minimum quantity | product  | unit rebate |
    | pasta-quantity | quantity | 2                | mno      | 0.45$       |
    And the following products are scanned:
    | sku | description   | unit price |
    | mno | pasta barilla | 2.95$      |
    | mno | pasta barilla | 2.95$      |
    When produce receipt
    Then the total is 5.00$

  Scenario: The one with a weight discount
    Given there is the following discount:
    | code          | discount | minimum weight | product  | unit rebate |
    | potato-weight | weight   | 1.00kg         | pqr      | 0.20$       |
    And the following products are scanned:
    | sku | description | unit price | weight |
    | pqr | potato      | 2.95$      | 2.00kg |
    When produce receipt
    Then the total is 5.50$

  Scenario: The one with a range discount
    Given there are the following discounts:
    | code           | discount | minimum weight | maximum weight | product  | unit rebate |
    | potato-range-1 | range    | 1.00kg         | 1.99kg         | pqr      | 0.20$       |
    | potato-range-2 | range    | 2.00kg         | 2.99kg         | pqr      | 0.25$       |
    And the following products are scanned:
    | sku | description | unit price | weight |
    | pqr | potato      | 2.95$      | 2.50kg |
    When produce receipt
    Then the total is 6.75$

  Scenario: The one with a "buy one get one free" discount
    Given there is the following discount:
    | code                  | discount        | product  |
    | pasta-buy-one-get-one | buy-one-get-one | mno      |
    And the following products are scanned:
    | sku | description   | unit price |
    | mno | pasta barilla | 2.95$      |
    | mno | pasta barilla | 2.95$      |
    When produce receipt
    Then the total is 2.95$

  Scenario: The one with a combo discount
    Given there is the following discount:
    | code    | discount | combo   | unit rebate |
    | hot-dog | combo    | stu,vwx | 0.50$       |
    And the following products are scanned:
    | sku | description      | unit price |
    | stu | hot-dog buns     | 1.25$      |
    | vwx | hot-dog sausages | 3.75$      |
    When produce receipt
    Then the total is 4.50$
```
</details>
