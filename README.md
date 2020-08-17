# Cash Register Kata

Let's build a cash register! Looks simple? Then let's add some constraints...

## The features

<details>
  <summary>Producing a receipt</summary>

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
</details>

<details>
  <summary>Pricing</summary>

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
