---
title: "Models, Views and Controllers"
permalink: "/docs/configuration/modelsviewscontrollers/"
---
## Model

The model is a representation of a database table.

The $this->rs array contains the fields of a particular table, it will initialise the database based on the initial value:

* $this->rs['col'] = 0; // Results in INTEGER

You can safely store numbers form -2147483648 to 2147483648 in an INTEGER field.

* $this->rs['col'] = 1.2; // Results in REAL

Use this type if you need to store float values or very large numbers.

* $this->rs['col'] = ''; // Results in VARCHAR(255)

Use this to store text up to 255 characters, if you need more, use MEDIUMBLOB

* $this->rs['col'] = array(); // Results in MEDIUMBLOB

A MEDIUMBLOB can be 16MB maximum.

You can override the creation of a field: add the field name to the $this->rt array and set the field create string. Take care to use a database agnostic string.
