#### Templates

a. Typed

    define(@co.dap.template) add(a co.lang.int, b co.lang.int)->(co.lang.int) ={
        this.return a +b;
    
    }


b. Untyped

    @co.dap.template
    define add(a,b)->(co.lang.untyped) ={
        this.return a+b;
    }

    Above is similar to C++ macros

c. Aren't templates are like functions?

     Yes templates are functions but inilined by default means the body is copy pasted at the place of calling so the scope is dynamic unlike in functions where scope is lexical/static.
     
     What it means?
        The variables in template are resolved (scope name type) at the place of calling rather than at declaration time
      



#### Constraints

     Templates must not be part of Libraries signatures/interfaces/module signatures.