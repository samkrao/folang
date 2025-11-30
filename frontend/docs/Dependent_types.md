## Dependent Types

   
    Dependent types allow types to depend on values, providing more precise type information and stronger compile-time guarantees.
    
    Language Support:
        
        1. Literal Singleton types 
           
            define identity( x co.lang.int) ->(x.type) = x

            where x.type is anonymous new type of co.lang.int it is a sub type also
            
            why it is useful

                you dont get exact value but also exact type

            eg
              
              identity(5) will return 5 but also exact type 5.type which is new type, subtype and anonymous type of int 

            How to use it

                if we use type inferencing only

                means ?
                  
                    define id = identity(5)

                    now what is id type ? it is 5.type which is subtype new type
                    and value ? 5

        2.Length Dependent types        

               define(@co.dap.dependentType(type="length")) x co.lang.int->([5]);

               what it is looking normal array? 

               yes it is normal array of ints with length 5

               now 

               define y co.lang.int->([7]);

               co.core.arrays.copy(y,x) // compiler will throw error saying lengths don't match
               
               We dont need to have leght checks null checks etc

               type system will take care of all these things  

        3. Opaque types

            Opaque types are a powerful type-system feature for creating strong abstraction boundaries

            define accountIdType co.lang.newtype = co.lang.int

            Now accountIdType is new type

            suppose 

               define getAccountDetails(a accountIdType)->(AccountDetails)={}

               if we pass getAccountDetails(3) compiler throws error as
               3's type int is not accountIdType

               define b accountIdType =3; okay as there is internal type constructor

               b.type is not same as 3.type

               rightway to pass
               getAccountDetails(accountIdType(3));

            define myInt co.lang.type = co.lang.int

            myInt is just alias

            suppose same funtion    
            
               define getAccountDetails(a myInt)->(AccountDetails)={}

               if we pass getAccountDetails(3) no issues
               
            
            Coming to opaque types

            define employeeIdType co.lang.opaquetype = co.lang.int
            define deptIdType co.lang.opaquetype=co.lang.int

            define getEmployeeDetails(a employeeIdType)->(EmployeeDetails)={}
            define getDeptDetails(a deptIdType)->(DepartmentDetails)={}

            getEmployeeDetails(3) works lik alias
            getDeptDetails(3) also works

            where it differs

            define d deptIdType = 3;

            now try to pass to getEmployeeDetails
            getEmployeeDetails(d) fails like new types
 
           
          
