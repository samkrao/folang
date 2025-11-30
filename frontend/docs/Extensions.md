## Extensions 

   #### use with care will not compile if it conflicts nature

   1. Addin a uppercase method for co.lang.string class
        
          define stringextensions co.lang.package ={
        
            
            @co.dap.extension(fortype=co.lang.string),what=extends)
            define  upperCase()->(string)={
                return this.upper()
            }
            
          }
    
   2. Override exiting things form classes

          define stringoverrides co.lang.package = {
            
              
              @co.dap.extension(fortype=[co.lang.string]),what=overrides)
              define equals(str string)->(bool)={
                this.return this == str
              }

          }


