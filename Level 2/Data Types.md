This is more of a reference page than instruction, but we'll start with the difference between signed and unsigned.
A signed data type simply refers to the presence of negative values within the range. 

Most of the basic C types only guarantee a minimum byte size, which is why it is best practice to use `sizeof()`, rather than just hardcoding a size when calling `malloc()`. 

## Common Types
### Char
Your smallest data type. Guaranteed to be at least 1 byte. You'll use this for Boolean values and possibly efficient storage of small numbers. This can be signed or unsigned, and I would recommend you specify in code for clarity and consistency. Also see [[Strings]].
*Since C99, there has been a `_Bool` data type, which you can use in modern compilers. This has a couple implications which can avoid rare pitfalls but otherwise you can use them interchangeably.*
### Int
Regular integer data type, guaranteed to have at least 16 bits but usually has more. If you want to write unsigned integer constant values, add a `u` suffix to your values, like this: `100u`. 
### Long
If you need a larger range of numbers. You can also have `long long`, which offers an even larger range, but that is unlikely to be necessary in most programs. Signed longs have the `l` suffix, and unsigned longs have the `ul` suffix. 
### Float
Floats are useful if you want decimal numbers. You must consider the loss in precision, which is normally negligible, but can sometimes trip you up. There's no signed/unsigned variant of these.
If you need a larger range float, consider using `double`, or in extreme cases, `long double`.
Floats don't have any minimum requirements, which should not usually be a concern, just if you're working on strange systems with esoteric hardware.
### Struct
See [[Structs]].


## Uncommon types
### Short
An integer type with more range than `char`, but less than `int`. 
### int8_t, uint8_t...
A selection of fixed width data types that you may use if you require a specific size variable. 
There are a couple variants available with different use cases, but the basic int8_t, uint8_t should do the trick for most applications. 



### sources
- [C data types - Wikipedia](https://en.wikipedia.org/wiki/C_data_types)
