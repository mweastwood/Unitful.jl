

<a id='Unitful.@u_str' href='#Unitful.@u_str'>#</a>
**`Unitful.@u_str`** &mdash; *Macro*.



```
macro u_str(unit)
```

String macro to easily recall units, dimensions, or quantities defined in the Unitful module, which does not export such things to avoid namespace pollution. Note that for now, what goes inside must be parsable as a valid Julia expression. In other words, u"N m" will fail if you intended to write u"N*m".

Examples:

```jlcon
julia> 1.0u"m/s"
1.0 m s^-1

julia> 1.0u"N*m"
1.0 m N

julia> typeof(1.0u"m/s")
Quantity{Float64, Dimensions:{𝐋 𝐓^-1}, Units:{m s^-1}}

julia> u"ħ"
1.0545718001391127e-34 J s
```


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/User.jl#L251-L276' class='documenter-source'>source</a><br>

<a id='Unitful.unit' href='#Unitful.unit'>#</a>
**`Unitful.unit`** &mdash; *Function*.



```
unit{T,D,U}(x::Quantity{T,D,U})
```

Returns the units associated with a quantity, `U()`.

Examples:

```jlcon
julia> unit(1.0u"m") == u"m"
true

julia> typeof(u"m")
Unitful.Units{(Unitful.Unit{:Meter}(0,1//1),),Unitful.Dimensions{(Unitful.Dimension{:Length}(1//1),)}}
```


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/Unitful.jl#L110-L126' class='documenter-source'>source</a><br>


```
unit{T,D,U}(x::Type{Quantity{T,D,U}})
```

Returns the units associated with a quantity type, `U()`.

Examples:

```jlcon
julia> unit(typeof(1.0u"m")) == u"m"
true
```


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/Unitful.jl#L129-L142' class='documenter-source'>source</a><br>


```
unit(x::Number)
```

Returns a `Unitful.Units{(), Dimensions{()}}` object to indicate that ordinary numbers have no units. This is a singleton, which we export as `NoUnits`. The unit is displayed as an empty string.

Examples:

```jlcon
julia> typeof(unit(1.0))
Unitful.Units{(),Unitful.Dimensions{()}}
julia> typeof(unit(Float64))
Unitful.Units{(),Unitful.Dimensions{()}}
julia> unit(1.0) == NoUnits
true
```


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/Unitful.jl#L146-L165' class='documenter-source'>source</a><br>

<a id='Unitful.ustrip' href='#Unitful.ustrip'>#</a>
**`Unitful.ustrip`** &mdash; *Function*.



```
ustrip(x::Number)
```

Returns the number out in front of any units. This may be different from the value in the case of dimensionless quantities. See [`uconvert`](conversion.md#Unitful.uconvert) and the example below. Because the units are removed, information may be lost and this should be used with some care.

This function is just calling `x/unit(x)`, which is as fast as directly accessing the `val` field of `x::Quantity`, but also works for any other kind of number.

This function is mainly intended for compatibility with packages that don't know how to handle quantities. This function may be deprecated in the future.

```jlcon
julia> ustrip(2u"μm/m") == 2
true

julia> uconvert(NoUnits, 2u"μm/m") == 2//1000000
true
```


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/Unitful.jl#L43-L67' class='documenter-source'>source</a><br>


```
ustrip{T,D,U}(x::Array{Quantity{T,D,U}})
```

Strip units from an `Array` by reinterpreting to type `T`. The resulting `Array` is a "unit free view" into array `x`. Because the units are removed, information may be lost and this should be used with some care.

This function is provided primarily for compatibility purposes; you could pass the result to PyPlot, for example. This function may be deprecated in the future.

```jlcon
julia> a = [1u"m", 2u"m"]
2-element Array{Quantity{Int64, Dimensions:{𝐋}, Units:{m}},1}:
 1 m
 2 m

julia> b = ustrip(a)
2-element Array{Int64,1}:
 1
 2

julia> a[1] = 3u"m"; b
2-element Array{Int64,1}:
 3
 2
```


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/Unitful.jl#L70-L98' class='documenter-source'>source</a><br>


```
ustrip{T<:Number}(x::Array{T})
```

Fall-back that returns `x`.


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/Unitful.jl#L101-L107' class='documenter-source'>source</a><br>

<a id='Unitful.upreferred' href='#Unitful.upreferred'>#</a>
**`Unitful.upreferred`** &mdash; *Function*.



```
upreferred(x::Number)
```

Unit-convert `x` to units which are preferred for the dimensions of `x`, as specified by the [`@preferunit`](newunits.md#Unitful.@preferunit) macro. If you are using the factory defaults in `deps/Defaults.jl`, this function will unit-convert to a product of powers of base SI units.


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/User.jl#L127-L136' class='documenter-source'>source</a><br>


```
upreferred(x::Units)
```

Return units which are preferred for the dimensions of `x`, which may or may not be equal to `x`, as specified by the [`@preferunit`](newunits.md#Unitful.@preferunit) macro. If you are using the factory defaults in `deps/Defaults.jl`, this function will return a product of powers of base SI units.


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/User.jl#L139-L148' class='documenter-source'>source</a><br>


```
upreferred(x::Dimensions)
```

Return units which are preferred for dimensions `x`. If you are using the factory defaults in `deps/Defaults.jl`, this function will return a product of powers of base SI units.


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/User.jl#L151-L159' class='documenter-source'>source</a><br>

<a id='Unitful.dimension-Tuple{Number}' href='#Unitful.dimension-Tuple{Number}'>#</a>
**`Unitful.dimension`** &mdash; *Method*.



```
dimension(x::Number)
dimension{T<:Number}(x::Type{T})
```

Returns a `Unitful.Dimensions{()}` object to indicate that ordinary numbers are dimensionless. This is a singleton, which we export as `NoDims`. The dimension is displayed as an empty string.

Examples:

```jlcon
julia> typeof(dimension(1.0))
Unitful.Dimensions{()}
julia> typeof(dimension(Float64))
Unitful.Dimensions{()}
julia> dimension(1.0) == NoDims
true
```


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/Unitful.jl#L169-L189' class='documenter-source'>source</a><br>

<a id='Unitful.dimension-Tuple{Unitful.Units{U,D}}' href='#Unitful.dimension-Tuple{Unitful.Units{U,D}}'>#</a>
**`Unitful.dimension`** &mdash; *Method*.



```
dimension{U,D}(u::Units{U,D})
```

Returns a [`Unitful.Dimensions`](types.md#Unitful.Dimensions) object corresponding to the dimensions of the units, `D()`. For a dimensionless combination of units, a `Unitful.Dimensions{()}` object is returned.

Examples:

```jlcon
julia> dimension(u"m")
𝐋

julia> typeof(dimension(u"m"))
Unitful.Dimensions{(Unitful.Dimension{:Length}(1//1),)}

julia> typeof(dimension(u"m/km"))
Unitful.Dimensions{()}
```


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/Unitful.jl#L193-L214' class='documenter-source'>source</a><br>

<a id='Unitful.dimension-Tuple{Unitful.Quantity{T,D,U}}' href='#Unitful.dimension-Tuple{Unitful.Quantity{T,D,U}}'>#</a>
**`Unitful.dimension`** &mdash; *Method*.



```
dimension{T,D}(x::Quantity{T,D})
```

Returns a [`Unitful.Dimensions`](types.md#Unitful.Dimensions) object `D()` corresponding to the dimensions of quantity `x`. For a dimensionless [`Unitful.Quantity`](types.md#Unitful.Quantity), a `Unitful.Dimensions{()}` object is returned.

Examples:

```jlcon
julia> dimension(1.0u"m")
𝐋

julia> typeof(dimension(1.0u"m/μm"))
Unitful.Dimensions{()}
```


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/Unitful.jl#L217-L235' class='documenter-source'>source</a><br>

<a id='Unitful.dimension-Tuple{AbstractArray{T<:Unitful.Units,N}}' href='#Unitful.dimension-Tuple{AbstractArray{T<:Unitful.Units,N}}'>#</a>
**`Unitful.dimension`** &mdash; *Method*.



```
dimension{T<:Units}(x::AbstractArray{T})
```

Just calls `map(dimension, x)`.


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/Unitful.jl#L248-L254' class='documenter-source'>source</a><br>

<a id='Base.:*-Tuple{Unitful.Unitlike,Vararg{Unitful.Unitlike,N}}' href='#Base.:*-Tuple{Unitful.Unitlike,Vararg{Unitful.Unitlike,N}}'>#</a>
**`Base.:*`** &mdash; *Method*.



```
*(a0::Unitlike, a::Unitlike...)
```

Given however many unit-like objects, multiply them together. Both [`Unitful.Dimensions`](types.md#Unitful.Dimensions) and [`Unitful.Units`](types.md#Unitful.Units) objects are considered to be `Unitlike` in the sense that you can multiply them, divide them, and collect powers. This function will fail if there is an attempt to multiply a unit by a dimension or vice versa.

Collect [`Unitful.Unit`](types.md#Unitful.Unit) objects from the type parameter of the [`Unitful.Units`](types.md#Unitful.Units) objects. For identical units including SI prefixes (i.e. cm ≠ m), collect powers and sort uniquely by the name of the `Unit`. The unique sorting permits easy unit comparisons.

Examples:

```jlcon
julia> u"kg*m/s^2"
kg m s^-2

julia> u"m/s*kg/s"
kg m s^-2

julia> typeof(u"m/s*kg/s") == typeof(u"kg*m/s^2")
true
```


<a target='_blank' href='https://github.com/ajkeller34/Unitful.jl/tree/f73bc51dd8c5dd9f645fd55c96e4fdc4ed14858e/src/Unitful.jl#L346-L374' class='documenter-source'>source</a><br>

