1. Class Declaration

        define myClass co.lang.class={
          
        }


    c. Ex:

        define Employee co.lang.class ={
          
            getEmployeeDetails()->(Employee) = empmodule.getEmployeeDetails; 
            //here it is assigning the module function to class's method
           
            getEmployeeInfo()->(Employee) =>> empmodule.getEmployeeDetails(); 
            //this is delegating means internally redirecting the call to module function 
        }

        $1 $2, $3 .... are the previous results capture in $[*] bind variable to pass to next
        methods or do something
        
        define Emp co.lang.class{

            
            dosomething(a co.lang.int, b co.lang.int)->(co.lang.int)=>>somePack.somMethod(a)=>>someOthPack.somOtherMeth($1,b);
            
            #What is $1 ?
            #$1,$2,$3 are result from previous method
        }

    d. Mixed methods

        define Employee co.lang.class ={

            (@co.dap.method.static) getEmployee()->(Employee) ={}
    
            (@co.dap.method.instance) getEmployee()->(Employee)={}
            
            (@co.dap.method.class)  getEmployee()->(Employee) ={}
    
            (@co.dap.method.object) getEmployee()->(Employee)={}
    
        }


    e. Inheritance 
    
        @co.dap.oops(

            A: { inherit:true,virtual:true},// classes
            B: {implements:true},  // interfaces
            C: {inherits:true,abstract=true}, //classes
            D: {inherits:true},  // classes
            E: {uses:true},  //mixins
            F: {composes:true},  //traits
            G: {extends:true},// extension classes
            H: {with:true} //behavior or capability kind
        )
        define test co.lang.class -> (uses=[],imlements=[],extends=[],inherits=[],with=package.type,composes=[]) ={

            getTest(id int)->(test) ={

            }

        }

    f. Different types for OOPS support
            
            i.   co.lang.mixin
            ii.  co.lang.class
            iii. co.lang.trait
            iv.  co.lang.interface
            v.   co.lang.abstract
            vi.  co.lang.virtual
            vii. co.lang.extension
            ix. co.lang.behavior
            x. co.lang.capability
