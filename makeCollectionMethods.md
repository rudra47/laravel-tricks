## macro()
The static macro method allows you to add methods to the Collection class at run time.
## Make Collection Methods
>Collections are "macroable", which allows you to add additional methods to the Collection class at run time. For example, the following code adds a toUpper method to the Collection class:
```
use Illuminate\Support\Collection;
use Illuminate\Support\Str;

Collection::macro('toUpper', function () {
    return $this->map(function ($value) {
        return Str::upper($value);
    });
});

$collection = collect(['fruits', 'vegetable']);

$upper = $collection->toUpper();

// Output = ['FRUITS', 'VEGETABLE']
```


