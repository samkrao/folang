
## Writing Performant programs

  1. Parallel

      Co lang programmer can use following annotations/directives to make multi processing 
            
            @co.dap.process    
         1. define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
   
            @co.dap.exec
         2. define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
            
            @co.dap.spawn    
         3. define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}

            @co.dap.fork
         4. define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
      
  2. Concurrent

      Co lang programmer can use following annotations/directives to achieve/introduce concurrency

         1. @co.dap.thread 
            define  doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
   
         2. @co.dap.task 
            define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
       
         3. @co.dap.fiber
            define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}

  3. Async

      Co lang programmer can use following annotations/directibes to write asyn programming

         1. @co.dap.async
            define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}

         2. @co.dap.coroutine
            define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
   
         3. @co.dap.generator
            define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
   
         4. @co.dap.subroutine
            define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
      
  4. Event 

      Co lang programmer can use following annotation(s)/directive(s) to create even driven programs.

         1. @co.dap.event
            define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
      
  
  5. continuations

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
      
      
  6. Call backs
  
     [Error and Exception handling](eeh.md)

  7. defer
       
       [Error and Exception handling](eeh.md)

 