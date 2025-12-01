
#Base types:
    co.lang.int
    co.lang.float
    co.lang.char
    co.lang.string
    co.lang.byte
    co.lang.bit
    co.lang.bool

#Literals:

    #IntegerLiteral
    #Bit Literal
    #Array Literal
    #Map Literal
    #String Literal
    #CharacterLiteral
    #Boolean Literal
    #Float Literal
    #ByteLiteral

#KeyWords/Reserved Words:

    co
    this
    define
    declare
    for
    impl
    let

#Operators

  [
  ]
  (
  )
  {
  }
  ==
  =
  !=
  =>>
  =>
  ->
  -
  +
  *
  /
  %
  $
  &
  #
  @
  !
  ~
  `
  ^
  |
  \
  /
  .
  ,
  ;
  :
  '
  "
  <=
  ||
  &&
  ...
  ..
  ??=
  ++
  +=
  -=
  --
  ~~
  __

#Syntax:

    #Variable declarations:
    
        #Simple:
            define x co.lang.int=10;
       
            @co.dap.mutable
            define x co.lang.int;

        #Pointer:
            define x co.lang.int->(*);
            define x co.lang.int->(**);
            define x co.lang.int->(***);

        #fat pointers and relative pointers

            define y co.lang.int->(*, meta={len:co.lang.usize,vtab:somepkg.VTable->(*)})
            define z co.lang.int->(*,kind=region, meta={})
            define z co.lang.int->(*,kind=relative, meta={})
        
        #Single Dimensional Array:
            define x co.lang.int->([5]);
        
        #Array of Arrays / Jagged Arrays:
            define x co.lang.int->([5][4]);
        
        #Multi Dimensional Array:
            define x co.lang.int->([5,4]);
        
        #Reference (LValue): 
            define x co.lang.int->(&);
                
        #RValue Referrences:
            define x co.lang.int->(&&);
        
        #Address Variables:
          
          
          #Signed:
        
               define y co.lang.intptr;
               define x co.lang.int->(@);
       
          #Unsigned:

               define z co.lang.uintptr; 

          #diff
           
               define p co.lang.ptrdiff;
              
        #Thunks:
            define x co.lang.int->(^);
        
        #Dynamic Arrays:
            define x co.lang.int->([...]);
            define x co.lang.int->([*]);
        
        #Zero Dimension Array:
            define x co.lang.int->([0]);
        
        #Single Dimension array size from initialization:
            define x co.lang.int->([])=[1,2,3,4];
        
        #JaggedArrays:
            define x co.lang.int->([[[4]]]); == define x co.lang.int->([4][][]);
        
        #pointer to Array:
            define j co.lang.int->((*)[4]); 
        
        #Array of Pointers:
            define k co.lang.int->(*[3]); 


    #Comma Statementts:
        define a co.lang.int;
        define b co.lang.int;
        define c co.lang.int;
   
        a,b,c;

        (a,b,c);

    #Function declarattion:

        define add (a co.lang.int, b co.lang.int)->(co.lang.int)={}
       
        define doNothing ()->()={}

        define multiret (a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.int){
            this.return (a+b, a-b);
        }

    #Types:

        #Alias:
            define x co.lang.type = co.lang.int 
        #New:
            define x co.lang.newtype=co.lang.int 

        #Opaque:
            define x co.lang.opaquetype=co.lang.int 

        #ADTs:
            define y co.lang.type=co.lang.int | co.lang.char  

        #Subtype or covariant:
            define test co.lang.subtype = co.lang.int;

        #supertype or Contravariant type:
            define test co.lang.supertype = co.lang.int;

    #Type Constructor:

        @co.dap.hokrt
        define Option(T) co.lang.type =  Some(T) | None();
    
    #UDTs:

        #Structs:
        
            define St1 co.lang.struct={
                
                name co.lang.string
                id co.lang.int
                age co.lang.float
            }

        #Type Associated functions

            define (St1) getName()->(co.lang.string)={
                return this.name   //herre this we can use as it is associatated to St1

            }

        #Struct signatures

            define Si1 co.lang.signature={
                getName()->(co.lang.string);
            }

            define St2 co.lang.struct={
                name co.lang.string
                id co.lang.int
                age co.lang.float
            }

            define (St2) getName()->(co.lang.string)={
                return this.Name
            }
          
            #Note: both Structs St1 and St2 implments the interface Si1 it is called behavioural #inheritance

            #Also if we not St1 and St2 are structurally compatible only works if all the fields #must match by number of fields, names and the types, order of fields doesn't matter

            #so 
              define s1 St1 = St1{name:"kamesh",age:50,id:1 };
              define s2 St2 = s1;

        #Strucural inheritance:

            define St1 co.lang.struct={
                name co.lang.string
                id co.lang.int
            }

            define St2 co.lang.struct={
                name co.lang.string
                id co.lang.int
                age co.lang.float
            }

            define s2 St2 = St2{name:"kamesh",id:1}
            define s1 St1 = s2 
            
            #possible as s1 is subset of s2
            
            #if s2 is s2 then Yes

        #Modules:
            define x co.lang.signature={}

            @co.dap.modulesig(x)           )
            impl mymod co.lang.module->(matches=x) = {
            }
        
        #Package:
            define x co.lang.package={}

        #Library:
           @co.dap.Library
           define x co.lang.package={

                @co.dap.export
                define SEmployee co.lang.signature={
                    declare Employee co.lang.struct;
                    declare storeEmployee ( emp Employee)->(Employee);
        
                }

           }
        
        #Class:
            #Interface

                define I co.lang.interface={
                    getAge()->(co.lang.float)
                }
            
            #Abstract Class:
                @co.dap.abstract
                define A co.lang.class={

                    getAge()->(co.lang.float)={
                        return 10.1;
                    }
                }

                @co.dap.abstract
                define A1 co.lang.class={

                    @co.lang.abstract
                    getAge()->(co.lang.float);
                }

                #inheriting abstract class must override all the abstract methods
                #Not instantiable
            
            #Virtual Methods:

                define A2 co.lang.class={
                
                    @co.lang.virtual
                    getAge()->(co.lang.float)={
                        return 10.1;
                    }
                }

                #inherriting this class must override all virtual methods
                #Instatiable
            
            #Sealed Class:

                @co.dap.sealed
                define C3 co.lang.class={

                }

                #cannot inherit class

            #Finalmethods
             
                define C4 co.lang.class={

                    @co.dap.final
                    getC4Name()->(co.lang.string)={
                        return "C4"
                    }
                }

            #Concrete:
            
                define x co.lang.class->(extends=[A,B],implements=[I,J,K])={
                    name co.lang.string
                    age co.lang.float

                    getAge()->(co.lang.float)={}

                }
                
                #Note: classes cannot be used for structural or behavioural inheritance
                #They are explicit inheritance

            #Method delegates:

                define Emp co.lang.class{

                    
                    dosomething(a co.lang.int, b co.lang.int)->(co.lang.int)=>>somePack.somMethod(a)=>>someOthPack.somOtherMeth($1,b);
                    
                    #What is $1 ?
                    #$1,$2,$3 are result from previous method
                }

    #Functors Applicatives Monads Monoids and Transformers:
      
        a. @co.dap.Functor
           define Functor (F)
    
           @co.dap.instance(typeclass=Functor, for=List)
           impl _ ->(instance=Functor(List)) 

        b. @co.dap.applicative
           define Applicative(F) ={
          
           @co.dap.instance(typeclass=Applicative, for=Option)
           impl _->(instance=Applicative(Option)) ={

        c. @co.dap.monad
           define Monad(F) ={
           
           @co.dap.instance(typeclass=Monad, for=Option)
           impl _->(insance=Monad(Option)) {
                
        d. @co.dap.monoid
           define  Monoid(T) ={
            
           @co.dap.instance(typeclass=Monoid, for=co.lang.int)
           impl _-> (instance=Monoid(co.lang.int)) ={   

        e. @co.dap.transformer
           define Transformer(F(_), G(_)) ={
            
           @co.dap.instance(typeclass=Transformer, for=[List,Set])
           impl _ -> (instance=Transformer(List(_), Set(_))) ={
  
    #Let Bindings:
    
        define y co.lang.int = let({$ =10}).in({$+1})
        define x co.lang.int=($ + 1).where($=10);

    #Auto infer:
        
        define x  =10;
        define x =(a co.lang.int)->(co.lang.int)=>10
    
    
    #Loops And Conditions

        ( boolean truth).do({ 

                }).otherwise(boolean truth).do({

                }).otherwise.do({

                });

        (boolean truth).loop({

                }).otherwise(boolean truth).loop({

                }).otherwise.loop({

                });

    #For
        
        define k co.lang.int = 1;
        for({

            k = k +1;
            co.out.println(k);
            (k >=10).do({
                this.break;
            });
        });
    
    #Pattern

        x.match.case(co.regex(\d+)=>co.const.true).default(co.const.false);
       
         x.match(co.pattern.Type).case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ")
       
        x.match(co.pattern.Value).case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ")
       
        x.match(co.pattern.Instance).case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ")
       
        x.match(co.pattern.Object).case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ")

        x.match(co.pattern.Shape).case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ")

       
        x.match(co.pattern.Any).case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ") 

         #default is co.pattern.Any
       

    #Ternary

        s =  (boolean truth).return (some var/value).otherwise.return(some val/var);

    #Iterable each

        var arr co.lang.int -> ([5]) = [6,7,8,9,10 ];
        a. arr.each(idx,val).do(
        b. arr.each(_,val).do(
    
    #Iterable Contains

        var arr co.lang.int->([5])=[35,57,96,81,31];
        var k co.lang.int = 31;
        arr.contains(k).do({})
    
    #Extensions:
         
        @co.dap.extension(fortype=co.lang.string),what=extends)
        define  upperCase()->(string)={
            return this.upper()
        }

        @co.dap.extension(fortype=[co.lang.string]),what=overrides)
        define equals(str string)->(bool)={
            this.return this == str
        }
    
    #Macros:

        @co.dap.macro
        define debug(expr)->(co.lang.untyped)

    #Templates

        @co.dap.template
        define add(a,b)->(co.lang.untyped) ={
    
        @co.dap.inline
        define add(a co.lang.int,b co.lang.int)->(co.lang.co.lang.int) ={
    
    #Annotations, Decorators, Directives, Pragmas:

     a. @co.dap.annotation
        define someAnn (...TypeParamSpec) = {
            
            define scope scope = scope.runtime  // or "compile"
            define process(ctx co.lang.annotationcontext)->()={}
            define before()->()={}
            define after()->()={}
            define around()->()={}
            define onError()->()={}

        }
    
     b. @co.dap.decorator
        define someAnn (...TypeParamSpec) = {
            
            define scope scope = scope.runtime  // or "compile"
            define process(ctx co.lang.context)->()={}
            define before()->()={}
            define after()->()={}
            define around()->()={}
            define onError()->()={}

        }

     c. @co.dap.pragma
        define someAnn (...TypeParamSpec) = {
            
            define scope scope = scope.runtime  // or "compile"
            define process(ctx co.lang.context)->()={}
            define before()->()={}
            define after()->()={}
            define around()->()={}
            define onError()->()={}

        }

     d. @co.dap.directive
        define someAnn (...TypeParamSpec) = {
            
            define scope scope = scope.runtime  // or "compile"
            define process(ctx co.lang.context)->()={}
            define before()->()={}
            define after()->()={}
            define around()->()={}
            define onError()->()={}

        }

    #Generics:

        @co.dap.generic (
            types={
                A: {variance:contravariant, bound: sometype1,default:som,nullable:true,Kind:Param},
                B: {variance:covariant, bound: sometype2, inclusive=false,Kind:Return},
                C: {variance:invariant, bound: sometype3,default:ss,Kind: Param},
                D: {variance:covariant, bound: sometype4,nullable=false,Kind:Param},
            }
        )
        define Transformer co.lang.function= ( A, C, D) -> ( B)

        @co.dap.generic(at=compile, 
            types={
                A: {types=[co.lang.int,co.lang.char,abc.Employee]},
                B: {types=[co.lang.float,co.lang.string]}
            }
        )
        define someMeth(a A,b B) -> ()


        @co.dap.generic(scope="runtime",where="callsite"
            types={
                A: {variance:contravariant, bound: sometype1,default:som,nullable:true,Kind:Param},
                B: {variance:covariant, bound: sometype2, inclusive=false,Kind:Return},
                C: {variance:invariant, bound: sometype3,default:ss,Kind: Param},
                D: {variance:covariant, bound: sometype4,nullable=false,Kind:Param},
            }
               
        )
        define myFun(a A, c C, d D)->(B)={}

        #What is where it is use site (declaration/callsite)


    #Indexer:

        define MyList co.lang.struct ={
              eles co.lang.int ->([*]);
        }
  
        @co.dap.indexer
        define (g MyList) [](index co.lang.int)->(co.lang.int) ={
           this.return g.eles[index]
        }

        @co.dap.indexer
        define (g MyList) []=(index co.lang.int, value co.lang.int) -> () ={
           g.ele[index] = value
        }

    #Operators:

        @co.dap.operator
        define + (a Employee, b Employee)->(Employee)={}

        @co.dap.operator(overload=true)
        define + (a Employee, b Employee)->(Employee)={}

        @co.dap.operator(override=true)
        define + (a co.lang.int, b co.lang.int) = {}

        @co.dap.operator(new=true)
        define ∪ (a co.lang.int, b co.lang.int)->(co.lang.int->([]))={}

         
    #Parallel:

        @co.dap.process    
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
   
        @co.dap.exec
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
            
        @co.dap.spawn    
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}

        @co.dap.fork
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
      

    #Conurrent:
        
        @co.dap.thread 
        define  doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
   
        @co.dap.task 
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
       
        @co.dap.fiber
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
    
    #Async:

        @co.dap.async
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}

        @co.dap.coroutine
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
   
        @co.dap.generator
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
   
        @co.dap.subroutine
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}

        @co.dap.goroutine
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
    
    #Continuations

        @co.dap.continuation
        define  doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}

        @co.dap.continuation(type=callcc)
        define  doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}

        @co.dap.continuation(type=promptcontrol)
        define  doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}

        @co.dap.continuation(type=shiftreset)
        define  doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}

        @co.dap.continuation(type=cps)
        define  doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}

    #Event:

        @co.dap.event
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}
    
    #Actors

        @co.dap.csp
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}

        @co.dap.actor
        define doSomeComplexLogic(a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.Error)={}

    #Lazy/lambda and anonymous

        #Anonymous

            @co.dap.anonymous 
            define _ (k co.lang.int)->(a co.lang.int) ={
            }(12);

        #Lambda:

            |x,y|=>  x+y 

        #Lazy

            @co.dap.lazy
            define x = add(1,2);

    #Callback and defer

        @co.dap.callback
        define myCallback(a co.lang.int, b co.lang.int)->()={}

        @co.dap.defer
        define myDeferred(a co.lang.int, b co.lang.int)->()={}


    #More functions

        #Inner function

            define myfun(a co.lang.int, b co.lang.int)->(co.lang.int)={
                var p co.lang.int = 10;
                define someother()->()={
                    co.out.println(p);
                }
                someother();
                p =20;
                someother();
            }        

        #Currying

            define curry(factor int)(val int)= { this.return factory * val};    

        #Default parameters
     
            define fun1 (k co.lang.int, b co.lang.char = 10)->(co.lang.int, co.lang.char)={
            }


        #Variable args

            define fun1 (k co.lang.int, ...b co.lang.char)->(co.lang.int, co.lang.char)={
            }

            **Curried functions cannot contain variadic args/optional args**

        #Optional Parameters

             define fun1(k? co.lang.int)->()={
                if k.omitted{

                }else{
                        
                }
            } 

        #Calling by Named args support

            define fun1(~k co.lang.int)->()={
                    
            } 

        #Closures:

            define add (x co.lang.int)->((co.lang.int)->(co.lang.int))={
                this.return (y co.lang.int)->(co.lang.int)={
                    this.return x + y;
                }
            }
       
        #Function with Function params

            define test(a (co.lang.int,co.lang.int)->(co.lang.int))->((co.lang.int)->(co.lang.int))={
                this.return a(1,2);
            }
    
    #Dependent Types

        define identity( x co.lang.int) ->(x.type) = x
    
    #Auto Infer at Compile time:

        define x co.lang.auto =10;
        define x =10

    #Dynamic / Duck typing

        define x co.lang.dynamic=10;
        x = 'A'
    
    #Value types
        define x co.lang.data=10;

        #type is not inferred at compile time but check from current value at runtime

        #Value dependant types

        #It is static becuase current values type mus match with new values type is like compile #time but actually things happens at runtime.
    
    #Native Functions:

         @co.dap.native   
         define  add (a co.lang.int, b co.lang.int)->(co.lang.int)={
    
    #Delegates
      
        @co.dap.delegate 
        define someDelegate co.lang.delegate = (a co.lang.int, b co.lang.int) -> (co.lang.int, co.lang.int );

    #Named Returns

        define doManythings(a co.lang.int, b co.lang.int->(&,meta={type=out}))->(r co.lang.int, e co.lang.excpetion)={}       

    #Bind Variables
    
      $[0-9]*
    
    #Discard/Wildcard Variable
       _

    
Where sentenses starrting with # are eitherr heading ,sub headings, and description regarding statements can use them as extra hints to Generate BNF/EBNF grammar to meet every statement without modifying anything no corrections KEEP EVERYTHING EXACTLY SAME
