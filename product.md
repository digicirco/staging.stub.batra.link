---
title: Product
---

{% assign p1 = site.data.products | first %}
{% assign tradeItem =  p1[1] %}
{% assign languageCode = "fr-BE" %}

# {% include translation.html translations=tradeItem.tradeItemDescriptions %}

<!-- TODO maybe don't show if same as tradeItemDescriptions -->
{% include translation.html translations=tradeItem.regulatedProductNames %}

<!-- TODO get code-->
{{ tradeItem.tradeItemClassification.gpcCategoryCode }}

[Batra link](https://www.batra.link/productFull.html?gtin={{ tradeItem.gtin }})

## Ingrédients : 

<!-- TODO remove "Ingredients:" at the beginning-->
{% include translation.html translations=tradeItem.ingredientInformation.ingredientStatements %}

{% include translation.html translations=tradeItem.healthRelatedInformation.compulsoryAdditiveLabelInformations %}

{% if tradeItem.allergenInformation.isAllergenRelevantDataProvided %}

## Allergènes : 

{% for allergen in tradeItem.allergenInformation.allergens %}{{ allergen.allergenTypeCode }}, {% endfor %}

{% endif %}


{% if tradeItem.nutritionalInformation.nutrientHeaders %}

## Nutritions

{% for nutrientHeader in tradeItem.nutritionalInformation.nutrientHeaders %}

### Pour {% include quantity.html quantity=nutrientHeader.nutrientBasisQuantity %}

{% for nutrientDetail in nutrientHeader.nutrientDetails %}

* {{ nutrientDetail.nutrientTypeCode }}: {% for quantityContained in nutrientDetail.quantitiesContained %}{% include quantity.html quantity=quantityContained %}, {% endfor %}

{% endfor %}

{% endfor %}

{% endif %}


## Contenu net: 

{% for netContent in tradeItem. tradeItemMeasurements.netContents %}
{% include quantity.html quantity=netContent %}
{% endfor %}

<!--- drainedWeight -->
<!--- tradeItemSize.descriptiveSizes -->

## Contact

{% for contact in tradeItem.contacts %}
{{contact.contactName}}, {{contact.contactAddress}}
{% endfor %}

## Origine

{% for countryOfOrigin in tradeItem.placeOfItemActivity.countriesOfOrigin %}
{{countryOfOrigin}}
{% endfor %}

{% if tradeItem.certificationInformation %}

## Instructions

{% include translation.html translations=tradeItem.consumerInstructions.consumerUsageInstructions %}

## Conservation

{% include translation.html translations=tradeItem.consumerInstructions.consumerStorageInstructions %}

{% for temperatureInformation in tradeItem.tradeItemTemperatureInformation.temperatureInformations %}
* {{temperatureInformation.temperatureQualifierCode}}: min {% include quantity.html quantity=temperatureInformation.minimumTemperature %}, max {% include quantity.html quantity=temperatureInformation.maximumTemperature %}
{% endfor %}

## Certifications

{% for certificationInformation in tradeItem.certificationInformation.certificationInformations %}
{{ certificationInformation.certificationStandard }} ({{ certificationInformation.certificationAgency }})
{% endfor %}

{% endif %}

<!--- preparationServings.preparationInstructions -->
<!--- alcoholInformation.percentageOfAlcoholByVolume -->
<!--- servingQuantityInformation.numberOfServingsPerPackage -->
<!--- nutriscores -->
<!--- isPackagingMarkedReturnable -->
