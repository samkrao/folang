1. Function Declaration
   
   
        define fun1 (k co.lang.int, b co.lang.char)->(co.lang.int, co.lang.char)={
        }

  
      
    
2. Curried Functions
   
   
        define add(first co.lang.int)(second co.lang.int)->(co.lang.int)={
        }


   
3. Function Closures

   a. As Statement
    
        define adder() -> ((co.lang.int) -> co.lang.int) ={
	     var sum co.lang.int = 0
	     this.return  (x co.lang.int) -> (co.lang.int) = {
		   sum += x
		   this.return sum
	     }
        }      
    
4. Anonymous / Lambda

 
   i. Anonymous functions   

     a.
        @co.dap.anonymous 
        define _ (k co.lang.int)->(a co.lang.int) ={
        }(12);
     
    
   ii. Lambda

       Only allowed as an inline callback argument to collection operations including arrays (e.g. map, filter, reduce, forEach, sortBy, groupBy, etc.). Using |...| anywhere else is a syntax/lint error.

       syntax |x,y|=>  x+y ;


       // collection use — allowed
        nums.map(|x| => x*x)
        words.filter(|s| => s.len() > 3)
        pairs.reduce(|acc, e| => acc + e, 0)
        dict.map(|k, v| => v * 10)
        list.sortBy(|a, b| => a.score - b.score)  // typical comparator callback

5. Inner Funtions

        define myfun(a co.lang.int, b co.lang.int)->(co.lang.int)={
            var p co.lang.int = 10;
            define someother()->()={
                co.out.println(p);
            }
            someother();
            p =20;
            someother();
        }
7. Function Types and Function Objects

     a.

        declare myobj co.lang.function =  (a co.lang.int, b co.lang.int)-> (co.lang.int)={
          this.return a + b;
        }

     b.

        define add (a co.lang.int, b co.lang.int)->(co.lang.int){ this.return a + b; }
        declare oObj co.lang.function = add;

     c.

        define funtype co.lang.type = (a co.lang.int, b co.lang.int)->(co.lang.int);

     d. define add co.lang.function =  (a co.lang.int, b co.lang.int)->(co.lang.int)=> a+b;
     
     *.

        a and b are same they are function objects only difference is in a the actual function is anonymous only associated with myobj

        c is function type when declared function type we can change the functions which satisfy the signature which is not the case for a and b
   
9. Others
  
   a.
     
        define closure(factor int) => (x int)=> x * factor;

   b.
    
        define curry(factor int)(val int)= factory * val;

   c. 
    
        define  curry2(x int) => (y: nt) = x + y

10. CoVariance Contra Variance and In Variant

     Variance is supported through generics/parametrics for normal function there is no Variance

     For more info please refer [GenericsParam](GenericsParam.md)                            


11. Optional/Variadic Parameters

    Language supports optional parameters or default values and variable arguments
    
     a. Default parameters
     
        define fun1 (k co.lang.int, b co.lang.char = 10)->(co.lang.int, co.lang.char)={
        }


     b. Variable args

        define fun1 (k co.lang.int, ...b co.lang.char)->(co.lang.int, co.lang.char)={
        }

        **Curried functions cannot contain variadic args/optional args**

     c. Optional Parameters

         define fun1(k? co.lang.int)->()={
                if k.omitted{

                }else{
                        
                }
         } 

     d. Calling by Named args support

         define fun1(~k co.lang.int)->()={
                
         } 

         Why explicit symbol?

         Please go through overloading functions to get better understanding
12. Multi Returns

     Language supports multiple returns
     
        define fun1 (k co.lang.int, b co.lang.char)->(co.lang.int, co.lang.char)={
        }

     by doing so has some restrictions 

     i. Even when there is no return we need to provide parenthesis as we do for parameters

           define fun1 (k co.lang.int, b co.lang.char)->()={
           }
       
           define fun1 ()->()={
           }
 
     ii.  Overloading supported based on return types, but restricted to internal not for developer
    
13. OverLoading

     Some restriction for functiton overloading

        i.  Curried functions cannot be used for overrloading or overloaded
        ii. inner functions (funcions in parent functions) cannot be used for overloading or overloaded
        iii. Lambdas/closure/anonymous functions cannot be used for overloading or overloaded
        iv. if a function name with
             a. default parameters/args
             b. optional parameters/args
             c. variadic parameters/args
             d. Allow named args
             e. function type as parameters and return types
        Cannot overload these type of functions

        What does cannot overload means

        If a function with name x has already defined with above then you cannot use same name x
        If a function with name x already defined with positional parameters cannot use same name x for above functions

        As these make language un necessarily complicate
        
        Bottom line what it means is a function with positional non variadic, required parameters with simple and composite types and return types (Excluding function types) only allowed to overload

14. Virtual/Overridable

      
      Language supports this kind of functions but wait they are not functions they are methods
      of class

      Virtual Function are annotated by @co.dap.virtual at symbol level

        define mycls co.lang.class={
                @co.dap.virtual
                myvirfun (a int, b int)->(int)= {return a + b}
        }

      It looks like normal function but it cannnot be used as is it should be overridden

      Subclasses needs to override them otherwise there will be compiler error while compliting sub class
      

15. Abstract

     It is similar to virtual function additionally no body will be there like function declaration

        @co.lang.abstract
        define mycls2 co.lang.class={
                @co.dap.abstract
                myabstrfun (a int, b int)->(int);
        }

        Additionally class to be annotated with abstract

        It follows normal oops concepts (no instance only inheritance)
16. New
    
    Some programming langugaes have same function name signature with new to make this is different from the previous and use this in the context where it declared


        annotation @co.dap.new
        
        ### Not supported Currently

        
        


17. Shadow

     Some programmming languges support this it is for static vs instance or private vs package vs public

        annotation is @co.dap.shadow
        
        ### Not suported currently


18. Function Delegates

      @co.dap.delegate 
      define someDelegate co.lang.delegate = (a co.lang.int, b co.lang.int) -> (co.lang.int, co.lang.int );

19. Function Chain

      define myarea (sideA co.lang.int, sideB co.lang.int)->(co.lang.int)=>>someModule.area(sideA,sideB);

      for class methods
        
        define fetchEmployee(empId co.lang.string )->(Employee)=>empMod.getEmploee(this,empId);

      What is this here
        
        this is the employee object we are passing

      Function are generic unlike class methods or type associated functions
   
        if we check the signature of getEmployee in empMod

      it should be something like this
   
        fun getEmployee(e empMod.Employee, empId co.lang.string)->empMod.Employee

   
      Similarly for type assoicated functions

        define (emp empMod.Employee) fetchEmployee(empId co.lang.string)->(empMod.Employee)=>empMod.getEmployee(emp, empId);

20. Advanced
   
     a.  [Native code](Functions_adv1.md)

21. Functions methods and type associated Functions

      1. Functions

           These are generic or common function till now what ever we discussed they apply to functions

           These are two types

               a. Package level

               b. Module level
                  
                     i. Virtual
                    ii. Shadowing
                   iii. new 
                    iV. abstract 
                     v. const (non modifiable)

               Rules are same all the above features and rules apply to functions no additional restrictions
      
      2. Class Methods

           Additional restrictions apply here

              a. No native (asm or iniline)
              b. No overload by return types
              c. No pointer ref, address, thunk or function parameters or returns always passed by ref
              d. No variadic args
              e. No default/optional parameters
              f. No call by named arg support
              g. No closures/currying
         
           Additional Features
             
              a. Class/instance/object/static type of methods
              b. public private protected type annotations
              

      3. Type associated functions 
            
           Additional restrictions apply here

              a. No native (asm or iniline)
              b. No overload by return types
              c. No pointer ref, address, thunk or function parameters or returns always passed by ref
              d. No variadic args
              e. No default/optional parameters
              f. No call by named arg support
              g. No closures/currying
            