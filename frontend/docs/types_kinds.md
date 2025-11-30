### Types and Kinds


define x co.lang.int = 10;

x.type() → int
x.kind() → undefined


define x co.lang.data = 10;

x.type() → Value
x.kind() → data
so to get actual type in folang
x.type().type() -> int
and it is static we can't assign x with another type at assignment type compiler will chek
x.type().type() with the value's type like co.lang.int/co.lang.auto

define x co.lang.auto = 10;

x.type() -> int
x.kind() ->data

//inferred at compile time and static

define x co.lang.dynammic = 10;

x.type() -> int
x.kind()-> data

here x type can vary 


define x (T co.lang.type)->(co.lang.type)= co.hokrt.Some(T) | co.hokrt.None();

x.type() → undefined
x.kind() → type->type


