## Function pattern parameter

   This is when a function accepts a pattern instead of a simple variable in its parameter list


    define f (Some(x)) =>{ x + 1 }
    define f (None()) => { 0 }

    converts to 

    define f(v) =>{
        v.match().case (x:Some(x) => x + 1).case (_:None()  => 0);
        }
