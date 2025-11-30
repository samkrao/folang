
1. Annotations

    These are the additional information provided to the compiler and/or runtime
   
      Level
      
        a. Global
        b. Symbol
        d. Keyword/ReservedWord
        e. Generic

      Examples:

         define dap package={
         
            define Variance co.lang.enum={
               covariant,
               contravariant,
               invariant
            }

              define scope co.lang.enum={
                  runtime,
                  comile,
               }
         

            define TypeParamSpec = {
                  Name string;
                  variance Variance;
                  bound string;
            }

            //Keyword level (attaching to define)
            @co.dap.annotation
            define someAnn (...TypeParamSpec) = {
               
               define scope scope = scope.runtime  // or "compile"
               define process(ctx co.lang.annotationcontext)->()={}
               define before()->()={}
               define after()->()={}
               define around()->()={}
               define onError()->()={}

               
            }
         }
       Annotations/decorators/pragmas/directives are similar to function objects but not functions
       so need to use decl keyword and type as one of the specified


    
    d. Other Types

       i.   @co.dap.directive
       ii.  @co.dap.pragma
       iii. @co.dap.decorator

    e. Others

       Aren't annotations looking like functions ?
           yes they are functions but the purpose is evaluate at compile time or run time depending on declaration prior to the symbol, keyword, the place at which it delcared

       Custom annotations integration

         1. pre hooks
            used for generating appropriate code and runs pre compile phase
         2. post hooks
            use for generating/emitting meta info others
