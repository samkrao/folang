Pattern Matching in the language 


    a. Simple pattern matching

        define x co.lang.int = 10;

        x.match.case(co.regex(\d+)=>co.const.true).default(co.const.false);

    
    b. Complex pattern matching


        define x co.lang.int = 10;

        x.match.case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ")
        

        where n is binding variable

        x.match(co.pattern.Type).case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ")
       
        x.match(co.pattern.Value).case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ")
       
        x.match(co.pattern.Instance).case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ")
       
        x.match(co.pattern.Object).case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ")

        x.match(co.pattern.Shape).case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ")

       
         x.match(co.pattern.Any).case(n: n > 10 => { n= n+100;"GT"}).case(_: n < 10 => "LT").default("EQ") //is default
       
##### _ is a special discard/ wildcard variable usable only inside pattern matching/ contains/iterators constructs (and similar discardable constructs), and is not a normal variable name by alone elsewhere _ must accompany by some ASCII letter or number .