### Macros Similar to Julia

   a.
    @co.dap.macro
    define  say()->()={
        this.return co.macro.quote({
            println("Line 1")
            println("Line 2")
        });
    }

    b.
    @co.dap.macro
    define yes_esc_assign()->(co.lang.untyped)={
        this.return co.macro.quote({
            co.macro.esc(y) = 42
            println("Inside macro: y = ", y)
        });
    }

    c.
    @co.dap.macro
    define debug(expr)->(co.lang.untyped)={
        let tmp = co.macro.gensym(co.lang.var,"tmp")   
                            //conflict with local variables after code generration
        this.return co.macro.quote({
            tmp = co.macro.esc(expr)
            println("Result: ", tmp)
            tmp
        });
    }

    d.

        if else condition macro
       
        @co.dap.macro(
            group = {items:["if","else"],chain:true},
            sugarform={forms:["if expr block"]},
            bind={vars:["x"]},
            isolate={vars:["temp", "index"]},  // → require gensym for those
            gensym={prefix:"tmp_"},           //  → set gensym naming strategy
            hygienic=true,                    //  → opt-out of hygiene manually
            argtransform={param:"body", wrap:"lambda",whentype:"block"},
            desugar={exprs:["if($cond) { $block }" => "if($cond,$block)"]},
            mode="inject"                   // # or "call" or "meta" or "inline" or "template"  
        
        )
        define if ( condition expr, body block)->()={
 
        }

        define blockormacro co.lang.Kind=  block | macro
        
        @co.dap.macro(
            group= {items:["if","else"],chain:true},
            sugarform={forms:["else block","else if"]},
            chainswith={macro:"if", position:"immediate", required:true},
            argtransform={param:"body", wrap:"lambda",whentype:"block"},
            standalone=false,
            desugar={exprs:[
              "else if($cond) { $block }" => "else(if($cond, $block))",
              "else { $elseblock }" => "else($elseblock)"
            ]},
        )
        define else (body blockormacro)->()={

        }




    e.

    Others

      1. @co.dap.compose(using=["base_if", "blockify"])
      2. @co.dap.guard(expr="is_bool_expr(expr)")
      3. Quasiquote Macros use co.macro.quote and co.macro.unquote


    f. Are macros are not similar to functions ?

         Yes they are but the compiler at compile time executes these functions and the result is added to AST instead of at runtime like functions and templates 

         If the changes to AST are applied at compile time is there a way to add things at runtime like javascript/python

             yes co.native.eval where you can pass any string it is run as code (data as code)





#### Constraints

     macros must not be part of Libraries signatures/interfaces/module signatures.