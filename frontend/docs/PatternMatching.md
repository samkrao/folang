Pattern Matching in the language 


    a. Simple pattern matching

        define x co.lang.int = 10;

        x.match.case(co.regex(\d+)=>co.const.true).default(co.const.false);

    
    b. Complex pattern matching


        define x co.lang.int = 10;

        x.match.case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ")
        

        where n is binding variable

        x.match(co.pattern.Type).case(co.lang.int   => ...).case(co.lang.float => ...);
        x.match(co.pattern.Value).case (0 => ...).case (1 => ...);
        x.match(co.pattern.Instance).case(xx.CAT=>...).case(xx.DOG => ...).default("Animal")
        x.match(co.pattern.Object).case(xx.Ball => "Ball").case(xx.CAT=> "CAT").default("Unknown")
            
        x.match(Shape).case (Point{x, y} => ...).default(_=> ...);
        
        Where object,instance and shape differ
            1. Functions objects are not instances
            2. Structs are pure types they are neither objects nor instances their instances are called shapes
        
        x.match(co.pattern.Any).(case co.lang.int   => ...).case (co.lang.float => ...).case (0 => ...).default(_=> ...);
        or
        x.match.case(co.lang.int   => ...).case (co.lang.float => ...).case (0 => ...).default(_=> ...);
        x.match(PositiveEvenMatcher).case(0   =>  "Neither even nor odd" ).case(2   =>"First Even Prime").default(...);
       
        Where PositiveEvenMatcher is customMatcher
        define Matcher(T) co.lang.signature = {
            matchCase(value T, pattern co.lang.untyped)
                -> (co.lang.int, MatchBindings); //int in return is number of matches 0 no match >0 match
        }
        @co.dap.matcher
        define PositiveEvenMatcher co.lang.shape = {
            matchCase(value co.lang.int, pat co.lang.untyped)->(co.lang.int, MatchBindings) = {
            // user logic…
            }
        }


##### _ is a special discard/ wildcard variable usable only inside pattern matching/ contains/iterators constructs (and similar discardable constructs), and is not a normal variable name by alone elsewhere _ must accompany by some ASCII letter or number .
