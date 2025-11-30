

1. Type Declaration        

    

        define str co.lang.struct={
        }

    

    Different Type kinds

          i. data => co.lang.data
         ii. type => co.lang.type
        iii. typeclass => co.lang.typeclass
         iV. container => co.lang.container
          V. instance=> co.lang.instance
         Vi. object => co.lang.object

    Difference between data and struct

          i. type cannot inherit from another type exception struct type, where it composes
               E.g:
                  define Employee co.lang.struct={
                          empId co.lang.int;
                          empName co.lang.string;
                  }
                  define Department co.lang.struct={
                          deptId co.lang.int;
                          deptName co.lang.string;
                  } 
                  define PermanentEmployee co.lang.struct{
                        Employee; //composition
                        Salary co.lang.float;
                        dept Department; //Association
                  }
         ii. While associating with functions

           For data

               
                define getEmployeeDetails(emp employee)->(employee) ={
                }
                
                calling 
        
                employee e;
                getEmployeeDetails(e);

           For struct

        define employee co.lang.struct={
             empno co.lang.int;
             empName co.lang.string;
        }

        define (emp employee) getEmployeeDetails()->(employee)={
        }

        or

        define getEmployeeDetails(emp employee)->(employee)={
        }

        employee e;
        e.getEmployeeDetails();
        or
        getEmployeeDetails(e);


2. Type Definition

   a. New type or invariant type
     
      define test co.lang.newtype = co.lang.int;
    
      test and co.lang.int have internally same representation but test != co.lang.int

      ex:
        
         define a test;
         define b co.lang.int;

         a.type == b.type  result false
    
    b. subtype or covariant type

         define test co.lang.subtype = co.lang.int;

    c. supertype or Contravariant type

         define test co.lang.supertype = co.lang.int;

3. Type Aliasing

    
    a.
      
        define test co.lang.type=co.lang.int;

        here test means co.lang.int

        define a test;
        define b co.lang.int;

        a.type == b.type  result true

4. ADT 
    
    i.  Union Types/Additive types

        define mytype co.lang.type= co.lang.int | co.lang.float;

    ii. Intersection Types

        define mytype co.lang.type= co.lang.int & co.lang.float;

    iii. Xor Types

        define mytype co.lang.type = co.lang.int ^ co.lang.float;


 5. Type Constructors

    @co.dap.hokrt
    define Option(T) co.lang.type =  Some(T) | None();
    //higher order type

    T -> Type
    K -> Kind
    V -> Value
    D -> Data
    O -> Order
    R -> Rank
    F -> Type constructor 
    
    @co.dap.hokrt
    define Ordering co.lang.type = Less() | Equal() | Greater();
    
    @co.dap.hokrt
    define Result(T, E) co.lang.type =  Ok(T) | Err(E);
    
    @co.dap.hokrt
    define Maybe(T) co.lang.type = Just(T) | Nothing();
    
    @co.dap.hokrt
    define Either(L, R) co.lang.type =  Left(L) | Right(R);


 6. Kind Constructor

    @co.dap.hokrt
    define Option(T : co.lang.kind) ->co.lang.kind=  //currently not supported
    //higher kinded type

  


 7. Tagged ADTS
    define Test co.lang.type =  co.hokrt.Int( co.lang.int ) | co.hokrt.Char( co.lang.char );




##### [Types and Kinds](types_kinds.md)
##### [Dependent Types](Dependent_types.md)





