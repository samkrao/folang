### Functors And Applicatives

    a. Functor

        1. example

            @co.dap.Functor
            define Functor (F) ={
                define map(value F(A), f (A)->B) -> (F(B));
            }

            @co.dap.instance(typeclass=Functor, for=List)
            impl _ ->(instance=Functor(List)) ={
                define map(value List(A), f (A)->B) ->( List(B)) ={
                    result = List(B){}

                    value.each(_,item).do({
                        result.append(f(item))
                    });

                    this.return result
                }
            }
            @co.dap.instance(typeclass=Functor, for=Set)
             impl _ ->(instance=Functor(Set)) ={
                define map(value Set(A), f (A)->B) ->( Set(B)) ={
                    result = Set(B){}

                    value.each(_,item).do({
                        result.append(f(item))
                    });

                    this.return result
                }
            }

    b. Applicative

        1. example
            
            @co.dap.applicative
            define Applicative(F) ={
                define pure(x A)-> (F(A));

                define apply(fab F(A->B), fa F(A))-> (F(B));
            }

            @co.dap.instance(typeclass=Applicative, for=Option)
            impl _->(instance=Applicative(Option)) ={

                define pure(x A) -> (Option(A)) ={
                    this.return Some(x);
                }

                define apply(fab: Option(A->B), fa: Option(A)) -> (Option(B))= {
                  this.return (fab, fa)
                        .match
                        .case((Some(f), Some(x)) => Some(f(x)))
                        .default(None());


                   
                }
            }

    c. Monods

        1. example
            
            @co.dap.monad
            define Monad(F) ={
                define pure(x A) -> (F(A));
                define flatMap(fa F(A), f (A)->F(B)) -> (F(B));
            }
            @co.dap.instance(typeclass=Monad, for=Option)
            impl _->(insance=Monad(Option)) {
                define pure(x A) -> (Option(A))= {
                    this.return Some(x);
                }

                define flatMap(fa Option(A), f (A)->Option(B)) -> (Option(B))= {
                  this.return fa.match().case(Some(x) => f(x)).default(None);
                   
                }
            }


    d. Monoid

        @co.dap.monoid
        define  Monoid(T) ={
            define empty() -> (T);
            define combine(a T, b T) -> (T);
        }
        @co.dap.instance(typeclass=Monoid, for=co.lang.int)
        impl _-> (instance=Monoid(co.lang.int)) ={
            define empty()->(co.lang.int) ={
                this.return 0;
            }

            define combine(a co.lang.int, b co.lang.int) ->(co.lang.int) ={
                this.return a + b;
            }
        }
       

    e. Transformer

        1. example
            
            @co.dap.transformer
            define Transformer(F(_), G(_)) ={
                define map(value F(A), f (A)->B) -> (G(B));
            }   
            
            @co.dap.instance(typeclass=Transformer, for=[List,Set])
            impl _ -> (instance=Transformer(List(_), Set(_))) ={
                define map(value List(A), f (A)->B) -> (Set(B)) ={
                    result = Set(B) {}
                    value.each(_,item).do({
                        result.insert(f(item))     
                    });

                    this.return result;
                }
            }

##### _ is a special discard/ wildcard variable usable only inside pattern matching/ contains/iterators constructs (and similar discardable constructs), and is not a normal variable name by alone elsewhere _ must accompany by some ASCII letter or number .


What is F(_) here _ is a wildard replaces any type of type