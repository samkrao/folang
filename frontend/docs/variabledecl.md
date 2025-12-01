# Variable Declaration

### var <varname> <type> [->(aditional qualification)] [=] [default value];

1. Simple Variable Declaration
   A Simple variable in fo-lang is a variable without any additional qualifications, the type can be fully qualified built in data type or UDT (user defined data type).
      
   a. Without initialization
   
        define b co.lang.char;
        define d co.lang.int ;
   
   b. With initialization
   
        define a co.lang.int=10;
   
3. Special Variable Declarations            

      a. Pointer Variable

           define pt co.lang.int->(*)=b;
           define ppt co.lang.int->(**)=pt; 
     
      b. Address Variable 
   
           define addr co.lang.int->(@)=a;

      c. Rerference Variable

            define ref co.lang.int->(&)=a;

      d.  RValue Reference

           define ref2 co.lang.int->(&&)=b;
            
      e. Arrays
   
           i. Single Dimension    

                define arr0 co.lang.int->([10]);
                define arr1 co.lang.int->([]) = [1,11,21,31]; //here the size is derived as 4 

           ii. Two Dimension

                 define arr1 co.lang.int->([2,3]);

          
           iii. Zero Dimension/Zero Length Arrays

                 define zdarr co.lang.int->([0]);

          iv. Jagged/Array of Arrays

                  define arr co.lang.int->([5][4]);
             
           v. Dynamic length or Flexible Array or VLA

                  define dynarr2 co.lang.int->([...]); // currently supported this
                  or
                  define dynarr1 co.lang.int ->([*]);
                  or
                  define famarr co.lang.int->(][);


           vi. Dynamic sized multi dimensional array
               
                  define jaggarr co.lang.int ->([[[3]]]);
                  //in java it is similar to declaring int  [][][] jaggarr =  new int[3][][];
                  //or int jaggarr1 [][][] = new int[3][][]
   
      f. Array of Pointers

          define k co.lang.int->(*[3]);
         
      g. Pointer to Array

          define j co.lang.int->((*)[4]);
      
      h. Thumks

          define k co.lang.int->(^)=10;

          A Thunk must be initialized.

          What are thunks ?
            
             Thunk variables are like any other variables as above but the value is not immediate.

             What do we mean by immediate ?

                 lets assume x =10;
                 what is the value of x 10 and it is immediate
                 means when we say x return 10

                 but thunks when we say x not valid
                 but when we say x.val return 10

             How it is different nothing more than additional indirection ?

                it differs not in direct values but

                if we say x  = a+b now immedate means a + b evaluated immediately
                but if we say variable is thunk then a + b is not evaluated until x.val is invoked

             Are thes not like lambdas ?

                yes it is similar to lambda but lambda is function and thunk is variable
                
                if x is lambda we need to invoke as x() and if x is thunk we should invoke as x.val

             Is it not like structs/ other custom types ?

                if we say x is struct and still the property val is immediate
                means

                type x struct {

                    val int = a +b; // means val is evaluated immediatelye
                }

              So,

               1. thunks are nullary means no arg lambda
               2. thunks are not callables but lambdas are callables (functions)

4. Additional Examples

    a. Use short name

        define k co.lang.int where co.lang.int is long name

        just use int ?

        some thing like this

        define k int;

        Yes it is possible

        @co.dap.builtinsshorthand
       
        define k int;

        @co.dap.builtinsshorthand is annotation/directive which must be declared as one of the toplevel directives

        What is top level directive ?

        A directive which is declared once for a file and not bound with any variables/functions etc.

        Caveats 

        If there is int type locally available it overrides the package specific things
        
        There is another annotation for shorthand user defined packages @co.dap.use(pkg="somepack",alias="")

        alias must be unique you can't use same alias for multiple packages.

    b. Implicit infer

        define k co.lang.int =10;

        Isn't better when we declare a vaiable with initialization then not give type and language automatically infer ?

        Yes

        to do that 

        define k auto = 10; //co.lang.auto 

        when we declare a variable with let we must initialize otherwise there is an error

        also remember k is always int type cannot take any other types. Only inferring is implicit

    c. Dynamic/DuckType

        Can we have a variable which take value of different datatypes something like in dynamic languages ?

        Yes.

        declare with

        define k dynamic = 10; k = "abc";  //co.lang.dynamic

        where type is dynamic/duck or runtime 

        Is it not nice if lang provide auto declaration of variables like some dynamic langugaes 

        Yes Sure,

        use
        @dap.autodeclare

        k =10; k = "abc";


        Again the directive @dap.autodeclare is toplevel directive not associated with any variables/functions one for file

        Disclaimer:
          Use it with utmost care as it will not produce any compiler errors for undeclared variables, no static analysis, even typos get un noticed

          

#### Advanced

     1. Fat Pointers

          define x co.lang.int->(*, meta={});

          define y co.lang.int->(*, meta={len:co.lang.usize,vtab:somepkg.VTable->(*)})

          define z co.lang.int->(*,kind=region, meta={})

               Pointer
               ├── base_type: T
               ├── kind: <FatKind>
               │    ├── thin
               │    ├── slice
               |    |── relative
               │    ├── trait
               │    ├── buffer
               │    ├── view
               │    ├── opaque
               │    ├── custom
               │    └── (region)  ← optional syntactic sugar
               └── meta:
                    ├── region: heap | stack | global | numa(N) | mmio | constant | …
                    ├── len, cap, vtab, bits, endian, …
     
     2. Integerpointers etc,

          a. Signed
        
               define y co.lang.intptr;

          b. Unsigned

               define z co.lang.uintptr; 
          
          c. Diff

               define p co.lang.ptrdiff;
     
     3. Relative Pointers

          a. define z co.lang.int->(*,kind=relative, meta={})


#### Some Limitations set by fo-lang while declaring Special Variables

     1.  Variable of type pointer are single pointer or double pointer only
     2.  Variables of type Multi Dimension arrays are of at max 4 dimensions
     3.  Variables of type Jagged Arrays/Array of Arrays are of at max 4 level similar to multi dimensions
     4. Pointer to Array or array of pointers must adhere to above conditions except pointer is only single pointer 
     5. Pointer and Ref (LValue) the difference is for pointer we can assign raw addresses unlike ref where it is assigned only by initializing with variable, otherwise both are one time assignment at the time of declaring (initialization is must) including Ref (Rvalue ) and address 
