---
description: A finer-grain version of Revit's Category system
---

# 4.12 Built-In Categories

#### Built-In Categories

The Revit user-interface shows many categories to users, such as Doors, Floors and Generic Models. However, Revit internally keeps track of a much more detailed list of categories, called Built-in categories. The full list of Built-in categories can be found in the [BuiltInCategory Enumeration](https://apidocs.co/apps/revit/2019/ba1c5b30-242f-5fdc-8ea9-ec3b61e6e722.htm) - these are hard coded and we can't create more of them.

#### Retrieving Elements of a BuiltInCategory

Because the list covers nearly 1000 categories, it helps programmers target Revit elements with much greater precision. Built-in categories can be especially useful for FilteredElementCollectors, letting us retrieve highly-specific kinds of elements. For instance, to collect all area tags in a document:

```python
#Boilerplate Code

area_tags = FilteredElementCollector(doc).OfCategory(BuiltInCategory.OST_AreaTags).ToElements()
OUT = area_tags
```

Combined with the usual [boilerplate code](../getting-started/boilerplate-setup-code.md), the above code will rapidly fetch all area tags in a document.  
It is this level of precision which makes BuiltInCategories especially useful when coding.


#### Comparing Category of an Element to a BuiltInCategory

Enums are at their core a list of number - string pairs, and in Revit the number (integer) part of the enum is used to create the Id of a Category. Ids that belong to built in elements in Revit are usually denoted with a negative number. 
To compare a category of an element to see if it is a certain BuiltInCategory follow these steps:
Get the integer id of the element Category, cast it into the type BuiltInCategory, compare that casted value to the BuiltInCategory you wish to check against:

```python
#Boilerplate Code
element = UnwrapElement(IN[0])
out=""

element_category_id=element.Category.Id.IntegerValue
casted_element_category=BuiltInCategory(element_category_id)

if casted_element_category==BuiltInCategory.OST_DuctCurves:
	out="Its a duct"
```
Because we are dealing with hard-coded values, these comparisons are language-independent.

#### Category Types

Categories themselves have another level of categorization. They are split into Model, Annotation, AnalyticalModel, Internal, Invalid. This enum can be accessed by the CategoryType property on the Category object.
Example how to check if an element belongs to a model category.

```python
#Boilerplate Code
element = UnwrapElement(IN[0])
out=""

element_category=element.Category
element_category_type=element.Category.CategoryType

if element_category==CategoryType.Model:
	out="This element belongs to a model category"
```
Example how to get all the model categories from the document, their BuiltInCategory and Names.

```python

model_cats=[]
model_cats_names=[]
model_cats_BuiC=[]

categories=doc.Settings.Categories

for cat in categories:
	if cat.CategoryType==CategoryType.Model:
		BuiC=BuiltInCategory(cat.Id.IntegerValue)
		model_cats.append(cat)
		model_cats_BuiC.append(BuiC)
		model_cats_names.append(cat.Name)
```
