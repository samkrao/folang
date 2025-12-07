
## Writing Performant programs

  1. Parallel

      Co lang programmer can use following annotations/directives to make multi processing 
            
         1. @co.dap.parallel    
            define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
          
  2. Concurrent

      Co lang programmer can use following annotations/directives to achieve/introduce concurrency
      
         1. @co.dap.concurent    
            define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
  
  3. continuations

      Co Lang supports two kinds of continuations.

         i. Full Continuations (Call/cc)
       
           a. Reusable
           b. Resumable
           c. Multi Shot
           d. CPS Style

         ii. Delimited (shift/reset, prompt/control,or trampolining/CPS) 
  

     Co Lang programmer can create continuations using following annotation/directive
     
         a. @co.dap.continuation
            define  doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
      
      
  4. Call backs
  
     [Error and Exception handling](eeh.md)

  5. defer
       
       [Error and Exception handling](eeh.md)

 