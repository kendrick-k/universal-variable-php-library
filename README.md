# Universal Variable PHP Library

A high level library for creating Universal Variable with PHP.

__Version: 1.0.1__

## Introduction

Universal Variable (UV) always requires some server-side development work to instantiate on each page of a website. At [Qubit](http://qubitproducts.com) we've created a PHP library to assist with this, thus requiring a little less 
intimate knowledge of client-side JavaScript.

This library is not prescriptive - it can be used to create the standard UV data layer, as well as extending it for more advanced and custom functionality.

Support the [Universal Variable Specification](http://docs.qubitproducts.com/uv/specification/) version 1.2.1.

Requires PHP version >= 5.2.0. [Find out more](#older-versions-of-php).


## Example

Create a page object within the UV and return json.

```php
$uv = new BuildUV();
$uv->get("page")->set("type", "category");
$uv->get("page")->set("breadcrumb", array("mens", "shoes"));

print $uv->toJSON();
=> {"page":{"type":"category","breadcrumb":["mens", "shoes"]},"user":{},"events":[]}
```


## Installation

Download the library directly from Github:

```
git clone git@github.com:QubitProducts/universal-variable-php-library.git
```

Include the `uv_library.php` file in any models/templates where you wish to instantiate UV.

### Older versions of PHP

PHP version >= 5.20 is required for the `json_encode` function. However, if you're on an older version of PHP you can enable this function by installing the [PECL package](http://pecl.php.net/package/json) directly.


## API

### Instantiation

Before you can run any methods, create the UV class:

```php
$uv = new BuildUV();
```

This automatically instantiates the standard specification object variables:

* page
* user
* product
* listing
* basket
* transaction
* events (array)


### get

Retrieve a variable within the UV. Works with any pre-declared variables, and further variables you may define

```php
$uv->get("page")
=> UVObject Object ( )

$uv->get("page")->get("category")
=> "Home"
```

The `get` method is usually only used to retreive a variable prior to setting one of its children.


### set

Set variables within the `product` object:

```php
$uv->get("product")->set("id", "12324214");
$uv->get("product")->set("name", "Sparkly Shoes");
$uv->get("product")->set("unit_price", 15);
```


### add 

Add a variable to an array. The following are the standard arrays found in UV:

* `listing.items`
* `basket.line_items`
* `transaction.line_items`
* `events`

```php

// Add event object to the events array
$uv->get("events")->add(array(
  "category": "checkout",
  "action": "step1"
));

// Add a line item object to the line_items array
$uv->get("basket")->get("line_items")->add(array(
  "product" => array(
    "id" => "12342332"
  ),
  "subtotal" => 10,
  "quantity" => 1
));
```

### toJSON

Outputs the UV object as json. Uses `json_encode`.

```php
print $uv->toJSON();
=> {"page":{"category":"Home","subcategory":"Mens - Shoes"},"user":{},"events":[]}
```

### toHTML

Outputs the UV object as json, declared as `window.universal_variable`, within `<script>` tags:

```php
print $uv->toHTML();
=>  <!-- Qubit Universal Variable data layer -->
    <script>
      window.universal_variable = {"page":{"category":"Home","subcategory":"Mens - Shoes"},"user":{},"events":[]};
    </script>
```


### Custom variables

If you'd like to add additional custom variables to the UV, you can do so by instantiating a new `UVObject` or `UVArray` class, and then run the methods as normal:

```php
// Object
$uv->my_custom = new UVObject();
$uv->get("my_custom")->set("id", "54321");

// Array
$uv->clicks = new UVArray();

// Add a string
$uv->add("click_button"); 

// Add an object
$uv->add(array(
  "category" => "signup",
  "action" => "button_click"
));
```




