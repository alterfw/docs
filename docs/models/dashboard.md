---
title: Dashboard customization
---

Using Alter you can customize the menu labels on WordPress dashboard:

```php
use Hero\Core\Model;
class Car extends Model {

  public $labels = ["Car", "Cars"];

}
```

You can also customize the icon that will appear on WordPress dashboard:

```php
use Hero\Core\Model;
class Car extends Model {

  public $icon = "dashicons-admin-home";

}
```

Alter uses the [WordPress Dashicons](https://developer.wordpress.org/resource/dashicons/#images-alt).
