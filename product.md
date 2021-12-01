---
title: Product
layout: none
---

{% assign languageCode = "fr-BE" %}

{% assign p1 = site.data.products | first %}
{% assign tradeItem =  p1[1] %}

{% assign gs1CodeValues = site.data.gs1Codes.Values_FR_3_1_16 %}

# {% include translation.html translations=tradeItem.tradeItemDescriptions %} ([lien](https://www.batra.link/productFull.html?gtin={{ tradeItem.gtin }}))

{% assign g1 = site.data.gpc.FoodBeverageTobacco_FR_2020_11 | where: "brickCode", tradeItem.tradeItemClassification.gpcCategoryCode | first %}
{% assign gpcBrick =  g1[1] %}

{{ g1.familyDescription }} > {{ g1.classDescription }} > {{ g1.brickDescription }}

<!-- TODO maybe don't show if same as tradeItemDescriptions -->
{% include translation.html translations=tradeItem.regulatedProductNames %}

## Ingrédients : 

<!-- TODO remove "Ingredients:" at the beginning-->
{% include translation.html translations=tradeItem.ingredientInformation.ingredientStatements %}

{% include translation.html translations=tradeItem.healthRelatedInformation.compulsoryAdditiveLabelInformations %}

{% if tradeItem.allergenInformation.isAllergenRelevantDataProvided %}

## Allergènes : 

{% assign allergenCodes = site.data.gs1Codes.Values_FR_3_1_16 | where: "listId","CNL3103" %}


{% for allergen in tradeItem.allergenInformation.allergens %}{% assign allergenCode = allergenCodes | where: "value", allergen.allergenTypeCode | first %}{{ allergenCode.name }} ({{allergen.levelOfContainmentCode}}), {% endfor %}

{% endif %}


{% if tradeItem.nutritionalInformation.nutrientHeaders %}

## Nutritions

{% assign nutritionTypeCodes = site.data.gs1Codes.Values_FR_3_1_16 | where: "listId","CNL3131" %}

{% for nutrientHeader in tradeItem.nutritionalInformation.nutrientHeaders %}

### Pour {% include quantity.html quantity=nutrientHeader.nutrientBasisQuantity %}

{% for nutrientDetail in nutrientHeader.nutrientDetails %}

{% assign nutritionTypeCode = nutritionTypeCodes | where: "value", nutrientDetail.nutrientTypeCode | first %}

* {{ nutritionTypeCode.name }}: {% for quantityContained in nutrientDetail.quantitiesContained %}{% include quantity.html quantity=quantityContained %}, {% endfor %}

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

<!-- ISSUE jekyll latest and on github pages to interpret csv string int the same way -->
{% assign countryOfOriginInt = countryOfOrigin | plus: 0 %}

{% assign countryLegacy = gs1CodeValues | where: "listId","CNL3112" | where: "value", countryOfOrigin | first %}
{% assign country = gs1CodeValues | where: "listId","CNL3112" | where: "value", countryOfOriginInt | first %}

{{ country.name | default: countryLegacy.name }}

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
