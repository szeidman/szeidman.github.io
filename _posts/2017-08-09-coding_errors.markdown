---
layout: post
title:  "Coding Errors"
date:   2017-08-09 14:07:31 +0000
---


## Some more details about the error handling in the Brewery app

One of the self-imposed quirks of my Rails Brewery app was the need for every Beer to have exactly four Ingredients. I mentioned a few of the challenges this presented in my previous post, but given the title of the blog it makes sense to focus a bit more on error handling, particularly for errors involving the custom attribute writer for an Ingredient amount.

## Creating a New Beer
### Beer Class

The custom attribute writer, on attempted creation of a new beer, allows assignment of four ingredients, one of each of four types (hops, malt, yeast, and water). On creating a new beer, there are four dropdown menus so the user can select an ingredient of each kind. The dropdown menus ensure that only previously created, valid ingredients are options, eliminating the potential for errors at the ingredient level here. However, the custom attribute, the amount for each ingredient, can still result in errors: what if the amount is blank, is not a number, or is a negative number?

To make sure such amounts would not be accepted as valid, I used an "amount_check" method and added it to validations (i.e., "validate :amount_check"). Here it is:

```
  def amount_check
    self.beer_ingredients.each do |beer_ingredient|
      if beer_ingredient.amount == nil
        errors.add(:base, "Amount for #{kind_for_amount(beer_ingredient)} can't be blank")
      elsif beer_ingredient.amount.to_f <= 0
        errors.add(:base, "Amount for #{kind_for_amount(beer_ingredient)} must be a number greater than zero")
      end
    end
  end
```

The method adds errors at the :base level for beer, iterating through each ingredient kind to see if the amount field should raise an error. It then uses "kind_for_amount", which finds the kind for the selected ingredient, to make clear in the error message which ingredient's amount is the source of the error:

```
def kind_for_amount(beer_ingredient)
    if Ingredient.find_by(id: beer_ingredient.ingredient_id)
      Ingredient.find_by(id: beer_ingredient.ingredient_id).kind
    else
      "ingredient"
    end
end
```

### The Beer form view

The above methods helped get the error messages rendering properly, but I also needed to have the amount field itself render as a field_with_errors if needed, and make sure that only the amount field or fields with errors rendered this way. Accordingly, in the Beer form partial I nested the label_tag for amounts in a div whose class would become field_with_errors depending on what base_beer_error returned:

```
<div class="<%= base_beer_error(ingredient_kind) %>">
   <%= label_tag "beer[ingredient_attributes][][amount]", "Amount (kg):", class: "col-sm-2" %>
</div>
```

The base_beer_error helper method went in the BeersHelper module. I figured the best way to light up the correct amount field would be to see if the error messages themselves included an ingredient's kind, like so:

```
  def base_beer_error(ingredient_kind)
    if @beer.errors[:base]
      if @beer.errors.messages[:base].any? {|message| message.include?(ingredient_kind)}
        "field_with_errors"
      end
    end
  end
```

This did the trick.

## Adding a new Ingredient to a Beer
I also needed to have similar validations for the nested New Ingredient form, used to add a new ingredient directly to an existing beer. Because this form only takes in one Ingredient at a time, these methods are a bit simpler because there is no need to keep track of which Amount field belongs to which ingredient.

### Ingredient class

In the Ingredient model, I made another amount_check validation method (since only one new Ingredient can be added to an existing Beer at a time, the error messages are slightly different):

```
  def amount_check
    self.beer_ingredients.each do |beer_ingredient|
      if beer_ingredient.amount == nil
        errors.add(:base, "Amount can't be blank")
      elsif beer_ingredient.amount.to_f <= 0
        errors.add(:base, "Amount must be a number greater than zero")
      end
    end
  end
```

### Ingredient form

To make field_with_errors appear in the right places in the views, I made a generic error_field? method: because there is only one Ingredient for this form, it sufficed to find a ":base" error in order to make the amount field render as field_with_errors.

Here is the relevant portion of the Ingredient form partial:

```
  <div class="<%= error_field?("base") %>">
      <%= label_tag "ingredient[beer][ingredient_attributes][][amount]", "Amount", class: "col-sm-1" %>
      </div>
```

And here is the helper method in IngredientHelper: 

```
  def error_field?(attribute)
    if @ingredient.errors[attribute.to_sym].count > 0
      "field_with_errors"
    else
      ""
    end
  end
```

Although it was more challenging to manage creating a Beer that needed four kinds of Ingredients on creation, the challenge led me to a more nuanced understanding of error handling than I would have gotten otherwise. After figuring out how to manage the Amount errors for four Ingredients at a time, it felt very straghtforward to manage the errors for a form that only submitted one Ingredient.
