# One-to-one

* This relationship is not as symetrical as it sounds,
  as there is still a primary and a secondary entity.
  The primary instance must exist before a secondary can be linked to it.
* The database implements a one-to-mamy relationship for a one-to-one
  model relationship.
  Care must be taken with the data integrity to ensure the one-to-one
  relationship holds.
  A unique index can ensure no primary instance has more than one secondary.
  *I'm not sure the need to create a unique key is actually documented.*
* The relationship can be set up so that a secondary can only exist with
  a primary (a `not null` foreign key), or it can be set up so
  a secondary can exist without a primary (a `nullable` foreign key).
  This second option is more symetrical, since an instance on either
  side of the relationship can exist without a matching instance on
  the other side.

## Forward Relationship

### ER Diagram

```mermaid
erDiagram
    Primary ||--o| Secondary : "hasOne()"
    Primary {
        mixed id PK
    }
    Secondary {
        mixed primary_id FK,UK
    }
```

### Relationship Method

```php
<?php

use Illuminate\Database\Eloquent\Collection;
use Illuminate\Database\Eloquent\Relations\HasOne;

/** @property Secondary|null $secondary */
class Primary extends Model
{
    public function secondary(): HasOne
    {
        return $this->hasOne(Secondary::class);
    }
}
```

## Inverse Relationship

### Inverse ER Diagram

```mermaid
erDiagram
    Secondary o|--|| Primary : "belongsTo()"
    Primary {
        mixed id PK
    }
    Secondary {
        mixed primary_id FK,UK
    }
```
### Inverse Relationship Method

```php
<?php

use Illuminate\Database\Eloquent\Relations\HasMany;

/** @property Primary $primary if the forign key is not null */
/** @property Primary|null $primary if the forign key is nullable */
class Secondary extends Model
{
    public function primary(): BelongsTo
    {
        // Omit the second parameter if this method name matches
        // the database foreign key column.

        return $this->belongsTo(Primary::class, 'primary_id');
    }
}
```
