# SemVer Converter

Converts SemVer version (like Composer packages) into integer version with operators. Helps managing versions: store, compare, sort and retrive by conditions.

Converter accepts the same versions as Composer, so You can use it with any package manager that accepts [Semantic Versioning](http://semver.org/).

# How it works?

For each version it uses Composer SemVer parser to parse version and normalize it. And then, for each version, converter creates array exploded by dot values. Each value multiply by 100 and create from it long string which is converted to integer. And for each version we have big integer.

### Sample:

1. Version **1.0.5**
2. Normalize with SevVer: **1.0.5.0**
3. Explode: **[ 1, 0, 5, 0 ]**
4. Multiply: **[ 100, 0, 500, 0 ]**
5. Converts to strings: **[ '100', '0', '500', '0' ]**
6. Relpace '0' with '000': **[ '100', '000', '500', '000' ]**
7. Concatenate all strings: **'100' + '000' + '500' + '000'**
8. Converts to integer: **(int) '100000500000'**

Result: **'1.0.5' == 100000500000**

# Examples

### Simple version

```php
$result = (new SemVerConverter)->convert('0.1.0');

// Result
array (size=1)
  0 => 
    array (size=2)
      'from' => 
        array (size=2)
          0 => int 100000000
          1 => string '==' (length=2)
      'to' => 
        array (size=2)
          0 => int 100000000
          1 => string '==' (length=2)
```

### Version between

```php
$result = (new SemVerConverter)->convert('>= 1.3.0 < 1.7.0');

// Result
array (size=1)
  0 => 
    array (size=2)
      'from' => 
        array (size=2)
          0 => int 100300000000
          1 => string '>=' (length=2)
      'to' => 
        array (size=2)
          0 => int 100700000000
          1 => string '<' (length=1)
```

### Tilde operator

```php
$result = (new SemVerConverter)->convert('~1.3');

// Result
array (size=1)
  0 => 
    array (size=2)
      'from' => 
        array (size=2)
          0 => int 100300000000
          1 => string '>=' (length=2)
      'to' => 
        array (size=2)
          0 => int 200000000000
          1 => string '<' (length=1)
```

### Version or

```php
$result = (new SemVerConverter)->convert('^1.9 || 3.0.*');

// Result
array (size=2)
  0 => 
    array (size=2)
      'from' => 
        array (size=2)
          0 => int 100900000000
          1 => string '>=' (length=2)
      'to' => 
        array (size=2)
          0 => int 200000000000
          1 => string '<' (length=1)
  1 => 
    array (size=2)
      'from' => 
        array (size=2)
          0 => int 300000000000
          1 => string '>=' (length=2)
      'to' => 
        array (size=2)
          0 => int 300100000000
          1 => string '<' (length=1)
```

# Licence

This code is licensed under MIT License.
