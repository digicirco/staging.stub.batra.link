---
title: Product
layout: none
---

| Flag                | Code | Region         |
|--|--|--|
| ![AD](png/EU.png)   | EU   | European Union |
| ![AE](png/WW.png)   | WW   | World          |
| ![CNA](png/CNA.png) | CNA  | North America  |
| ![CSA](png/CSA.png) | CSA  | South America  |
| ![CEU](png/CEU.png) | CEU  | Europe         |
| ![CAF](png/CAF.png) | CAF  | Africa         |
| ![CAS](png/CAS.png) | CAS  | Asia           |
| ![COC](png/COC.png) | COC  | Oceania        |

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

<!-- ISSUE jekyll latest and on github pages to interpret csv string int the same way -->

**Origine:** {% for countryOfOrigin in tradeItem.placeOfItemActivity.countriesOfOrigin %}{% assign countryOfOriginInt = countryOfOrigin | plus: 0 %}{% assign countryLegacy = gs1CodeValues | where: "listId","CNL3112" | where: "value", countryOfOrigin | first %}{% assign country = gs1CodeValues | where: "listId","CNL3112" | where: "value", countryOfOriginInt | first %}{{ country.name | default: countryLegacy.name }},{% endfor %}

**Contenu net:** {% for netContent in tradeItem. tradeItemMeasurements.netContents %}{% include quantity.html quantity=netContent %}{% endfor %}

{% if tradeItem.certificationInformation %}**Certifications:** {% for certificationInformation in tradeItem.certificationInformation.certificationInformations %}{{ certificationInformation.certificationStandard }} ({{ certificationInformation.certificationAgency }}),{% endfor %}{% endif %}

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

{% assign nutrientHeaders = tradeItem.nutritionalInformation.nutrientHeaders %}

||{% for nutrientHeader in nutrientHeaders %}Pour {% include quantity.html quantity=nutrientHeader.nutrientBasisQuantity %}|{% endfor %}
|--|{% for nutrientHeader in nutrientHeaders %}--|{% endfor %}
|test|test|test|{% for nutritionTypeCode in nutritionTypeCodes %}{% for nutrientHeader in tradeItem.nutritionalInformation.nutrientHeaders %}{% assign nutrientDetail = nutrientHeader.nutrientDetails | where: "nutrientTypeCode", nutritionTypeCode.value | first %}{% if nutrientDetail %}{% if forloop.first == true %}
|{{ nutritionTypeCode.name }}|{% endif %}{% for quantityContained in nutrientDetail.quantitiesContained %}{% include quantity.html quantity=quantityContained %},{% endfor %}|{% endif %}{% endfor %}{% endfor %}

{% endif %}

<!--- drainedWeight -->
<!--- tradeItemSize.descriptiveSizes -->

## Instructions d'utilisation

{% include translation.html translations=tradeItem.consumerInstructions.consumerUsageInstructions %}

## Instructions de stockage

{% include translation.html translations=tradeItem.consumerInstructions.consumerStorageInstructions %}

## Températures

{% for temperatureInformation in tradeItem.tradeItemTemperatureInformation.temperatureInformations %}
* {{temperatureInformation.temperatureQualifierCode}}: min {% include quantity.html quantity=temperatureInformation.minimumTemperature %}, max {% include quantity.html quantity=temperatureInformation.maximumTemperature %}
{% endfor %}

## Contact

{% for contact in tradeItem.contacts %}
{{contact.contactName}}, {{contact.contactAddress}}
{% endfor %}

<!--- preparationServings.preparationInstructions -->
<!--- alcoholInformation.percentageOfAlcoholByVolume -->
<!--- servingQuantityInformation.numberOfServingsPerPackage -->
<!--- nutriscores -->
<!--- isPackagingMarkedReturnable -->

<!--
Durée de vie :

Durée de conservation sortie d'usine :
150 jour(s)
Durée minimum de conservation à l'arrivée :
100 jour(s)
Durée de conservation après ouverture :
5 jour(s)
-->
