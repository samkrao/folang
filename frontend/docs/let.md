### special keyword let

   Let is for let bindings in folang, it is for variables not for functions like (ML) languages, as functions are recursive by nature like any other mainstream languages.

Example:

   1. define y co.lang.int = let({x =10}).in({x+1});
   2. define y co.lang.int = let({$ =10}).in({$+1});
   

   3. define x co.lang.int= (x+1).where(x=10);
   4. define x co.lang.int=($ + 1).where($=10);

##### $ is a special bind-variable usable only inside let / where-style constructs (and similar binding constructs), and is not a normal variable name elsewhere.