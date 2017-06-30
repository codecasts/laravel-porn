## Eloquent relationships with JSON fields.

Wanna store some data in MySQL / PostgreSQL JSON fields but is afraid of losing the awesome relationship capabilities? 


Don't worry we got you covered:

### Scenario:

1) Your `User`'s model has a `address` JSON field that holds all the users information, inclusing the address City. 
2) You have a `City` model that holds all cities of your country.
3) Inside the user `address` field, there is a json field holding the `city_id`

### Definition

```php
<?php

namespace LaravelPorn;

use Illuminate\Database\Eloquent\Model;

/**
 * Just a boring normal city model.
 */
class City extends extends Model
{
    protected $table = 'cities';
}

```

```php
<?php

namespace LaravelPorn;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    protected $table = 'users';
    
    /**
     * You need to cast the address field as array, so the JSON will be always an array
     */
    protected $casts = [
        'address' => 'array'
    ];
    
    /**
     * This is the actual trick. the city_id field is a virtual one.
     */
    public function getCityIdAttribute()
    {
        return $this->address['city_id'] ?? null;
    }
    
    /**
     * Finally the relationship.
     * The city_id field used here is the accessor, it does not exists on the table, but
     * only inside the json field address
     */
    public function city()
    {
        return $this->belongsTo(City::class, 'city_id')
    }
}

```

### Usage

Enjoy!

```php

$user->city; // LaravelPorn\City

```
