daffodil
========

data filtering lib

[![build status](https://travis-ci.org/mediapredict/daffodil.png)](https://travis-ci.org/mediapredict/daffodil)

For 18 year old women:

```
age = 18
gender = "female"
```

For men, 18 - 35:

```
gender = "male"
age >= 18
age <= 35
```

(this is what we'll use for MP)

You can do OR rules instead of AND rules using "[]"  

People who are 18 years old or 21 years old:

```
[
  age = 18
  age = 21
]
```

Women who are 18 or 21:

```
gender = "female"
[
  age = 18
  age = 21
]
```

This also works:

```
gender = "female"
[ age = 18, age = 21 ]
```

"{}" means "AND"

Women who are 18 or 21, or between 35 and 55:

```
gender = "female"
[
  age = 18
  age = 21
  {
    age >= 35
    age <= 55
  }
]
```

Quotes around the Field names are optional when the name is only letters, numbers, dashes, and underscores. The following three examples are all exactly equivalent:

```
gender = "female"
```

```
"gender" = "female"
```

```
'gender' = "female"
```

## "NOT" operator

Maybe applied to either "AND" or "OR".
Used with "OR" will match "all those not being 18 or 21"

```
![
  age = 18
  age = 21
]
```

When used with "AND" it will match all except women being 18

```
!{
  age = 18
  gender = "female"
}
```

## Arrays

Allows "IN" lookup. So:

```
age in (20, 30, 40)

```
... is the same as:
```
[
  age = 20
  age = 30
  age = 40
]
```

"NOT IN" may be applied in the following way:
```
age !in (20, 21, 22)
```
and it will match all the people except those being 20, 21 or 22 years.

Different formats are allowed, so the following 2 examples are equivalent as the above, either separated with a newline:
```
age !in (
  20
  21
  22
)
```
or mixed:
```
age !in (
  20, 21
  22
)
```

Apart from `integers`, array may contain `decimals` or `string` as long as all its elements are of the same type:
```
"price" in (9.95, 10.95, 11.95)
```
or
```
"color" in ("blue, "red", "green)
```
This ***WOULDN'T work***:
```
age in (20, "21", 22, 23)
```


## Empty "AND" Blocks and "OR" blocks

An empty "AND" block will match **all** users

```
{}
```

An empty "OR" block will match **no** users

```
[]
```

This means you can set `{}` as your last filter in cases where you want to match various groups and then have an "everybody else" group.


REFERENCE
=========

### Value types

Numbers: 
- `"x" = 1`
- `"x" = 500000`
- `"x" = 7.5`
- `"x" = -1642`
- `"x" = -0.9141`
 
Strings:
- `"x" = "hello"`
- `"x" = "a complete sentence"`
- `"x" = 'this time with single quotes'`
- `"x" = 'double quotes "inside" single quotes'`
- `"x" = 'single quotes 'inside' double quotes"`
- `"x" = 'uniçode ís tøtålly fiñe'`

Booleans:
- `"x" = true`
- `"x" = false`

Arrays:
- `"x" in (12, 13, 14)`
- `"x" in ("red", "green", "blue")`

### Comparison operators

Operator | Example | Meaning
---|---|---
Equal | `"x" = 100` | `x` is `100`  
Not Equal | `"x" != 100` | `x` is not `100`  
Less Than | `"x" < 100` | `x` is less than `100`  
Greater Than | `"x" > 100` | `x` is more than `100`  
Less than or Equal | `"x" <= 100` | `x` is less than or equal to `100`  
Greater than or Equal | `"x" >= 100` | `x` is greater than or equal to `100`  
Exists | `"x" ?= true` | `x` has any value (where it exists)
Exists | `"x" ?= false` | `x` has no value (where it does not exist)
Not Any | `!["x"=5, "x"=6]` | `x` has any value except 5 or 6 (or it does not exist)
Not All | `!{"x"=2, "y"=3}` | exclude where both `x` is `2` AND `y` is `3`
In | `x in (5, 6)` | `x` is either 5 or 6
Not In | `x !in (5, 6)` | `x` has any value except 5 or 6 (or it does not exist)
