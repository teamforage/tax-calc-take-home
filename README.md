# tax-calc-take-home

## Instructions
You will be writing a tax calculation engine that Forage API consumers could use to determine the amount of taxes to charge an order that is being paid for with SNAP. This exercise shows you a challenge Forage has solved for our customers. Feel free to implement your solution in whichever language you feel most comfortable with. To get started,

1. Clone this repository.
1. Implement the Required Solution below.
1. Push your code changes to a private repository hosted by you on github. 
1. Email your interviewer to invite them to your private repo and to schedule a code review.
1. Do a live code review with your interviewer. 

## Background Information
The USDA FNS governs the [SNAP](https://www.fns.usda.gov/snap/supplemental-nutrition-assistance-program) program and has a number of restrictions in place around spending SNAP:
1. SNAP can only be used for household food products such as fruits, vegetables, meat, poultry, dairy products, breads, and other foods such as snack foods and non-alcoholic beverages. SNAP cannot be used for alcohol, foods that are hot at point-of-sale, and any non-food items.
2. No taxes can be charged on an item that is paid for with SNAP. If an item is paid for partially by SNAP, the remaining portion should be taxed.
3. SNAP should be applied to a cart so that the minimum amount of taxes are charged to the customer.

### Required solution
The goal of this exercise is to ingest a list of products and return a breakdown of the taxes that were applied to each product, as well as the total taxes charged. SNAP can only be charged to items with `"eligibility": "snap"`, so all items with `"eligibiilty: "ebt_cash"` should be fully taxed (EBT Cash is another form of benefit). Remember, SNAP should be applied to a cart so that the minimum amount of taxes are charged to the customer. Products can come in two different forms based on the granularity of the tax rates passed in. The list of products can include both products with a single `tax_rate` field and products with a `tax_rate_list` field.

Here is an example of a product with a single `tax_rate`:
```json
{
  "name": "Apples",
  "upc": "1",
  "unit_price": 5.99,
  "tax_rate": 0.06,
  "quantity": 3,
  "eligibility": "snap",
  "taxes_charged": null
}
```

Here is an example of a product with tax rates broken down by source in a `tax_rate_list` field:
```json
{
  "name": "Apples",
  "upc": "1",
  "unit_price": 5.99,
  "tax_rate_list": [
    {
      "imposed_by": "Los Angeles County Tax",
      "tax_rate": 0.015,
      "taxes_charged": null
    },
    {
      "imposed_by": "California State Tax",
      "tax_rate": 0.045,
      "taxes_charged": null
    }
  ],
  "quantity": 3,
  "eligibility": "snap",
  "taxes_charged": null,
}
```

You will implement a function that takes in a list of product objects and the amount of SNAP that should be applied to the order. The function should return the total amount of taxes charged and an updated list of product objects with all `taxes_charged` fields filled in. Here is an example:
```typescript
function calculateTaxes(
  productList: Product[],
  snapPaid: number
): { accumulatedTaxes: currency; updatedProductList: Product[] } {
  ...
}

const productList = [
  {
    "name": "Apples",
    "upc": "1",
    "unit_price": 5.99,
    "tax_rate_list": [
      {
        "imposed_by": "Los Angeles County Tax",
        "tax_rate": 0.015,
        "taxes_charged": null
      },
      {
          "imposed_by": "California State Tax",
          "tax_rate": 0.045,
          "taxes_charged": null
      }
    ],
    "quantity": 3,
    "eligibility": "snap",
    "taxes_charged": null,
  }
]

const calculatedTaxesObject = calculatedTaxes(productList, 10)
```

The code snippet above would return total taxes charged of 0.48 and an updated list of products:
```json
[
  {
    "name": "Apples",
    "upc": "1",
    "unit_price": 5.99,
    "tax_rate_list": [
      {
        "imposed_by": "Los Angeles County Tax",
        "tax_rate": 0.015,
        "taxes_charged": 0.12
      },
      {
          "imposed_by": "California State Tax",
          "tax_rate": 0.045,
          "taxes_charged": 0.36
      }
    ],
    "quantity": 3,
    "eligibility": "snap",
    "taxes_charged": 0.48,
  }
]
```

Please create a `taxCalculation` file implementing your solution.

### Testing
Your solution should be unit tested based on the json objects found in the fixtures folder and any further scenarios you think would be good to test. Here are 4 test scenarios that should be included in your testing suite:
1. `ebt_cash_only_product_list.json` and $20 of SNAP
- Total amount of taxes charged: $1.60
2. `snap_only_product_list.json` and $11.33 of SNAP
- Total amount of taxes charged: $1.23
3. `mixed_product_list.json` and $32
- Total amount of taxes charged: $1.61
4. `mixed_product_list_tax_rate_list.json` and $16.72
- Total amount of taxes charged: $2.30

Please create a `tests` file that imports your solution and the json objects and unit tests your implementation.
