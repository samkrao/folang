## Reserved Words

   1. declare
   2. define
   3. co
   4. this
   5. for
   6. impl
   7. let


### declare

   CO language provides this keyword/reserved word for declaring externally provided things

   for e.g., declare Employee co.lang.struct means there is Employee type made available at runtime

## define
  
   CO language provides this keyword/reserved word to define variables functions classes types etc..


   for e.g

   define k co.lang.int;

   define Employee co.lang.struct={}

   define add (a co.lang.int, b co.lang.int)->(co.lang.int)={}


### co

   CO Language wraps  default implementations of supported features under this

        1. built in methods
        2. built in statements and expressions
        3. built in data types
        4. type declaration and kinds

   for e.g.,

   co.out.println

   co.lang.int

   co.call

   co.async

   co.thread etc.

### this

    is the keyword available every where from simple block to functitons classes everywhere refers current object 

    Everything in co is object and every thing is functional including statements or expressions

### for

    infinite loop is function takes block of code 

### impl

   used for creating module implementations and trait implementations

### let

   used for non recursive let bindings as recursive is functions and recursive functions already supported.