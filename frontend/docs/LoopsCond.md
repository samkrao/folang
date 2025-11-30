# Loops And Conditional Statments/Expressions

    a. Conditions And loops
   
        i . Conditions
   
            ( boolean truth).do({ 

            }).otherwise(boolean truth).do({

            }).otherwise.do({

            });
   
        ii. loops
   
            (boolean truth).loop({

            }).otherwise(boolean truth).loop({

            }).otherwise.loop({

            });

        iii. Condition and Loop mix.
   
            (boolean truth).do({

            }).otherwise( boolean truth).loop({

            }).otherwise(boolean truth).do({

            }).otherwise.loop({

            });
    
    b. Ternary Operator

         i. s =  (boolean truth).return (some var/value).otherwise.return(some val/var);
        ii. s = (boolean truth).return (some var/val).othrewise(boolean truth).return (some var/val).otherwise.return(some var/val); 
   
    c. Breaking / continuing loop

         i. this.break;
        ii. this.continue;

    e. Dummy Statement

       i. co.nop;

    f. Looping arrays/lists/maps/ranges
   
       i. var arr co.lang.int -> ([5]) = [6,7,8,9,10 ];
          arr.each(idx,val).do({
                 co.out.print(idx);
                 co.out.print(" :: ");
                 co.out.println(val);
           });
   
       ii. var arr co.lang.int->([5])=[22,33,41,12,98];
           arr.each(_,val).do({
                 co.out.println(val);
          });
   
    g. Array/List/Map/Range contains Element

       i. var arr co.lang.int->([5])=[35,57,96,81,31];
          var k co.lang.int = 31;
          arr.contains(k).do({
                co.out.println(k);
          }).otherwise.do({
                co.out.println("Not Found");
          });

       ii. var arr co.lang.int->([5])=[11,31,21,64,56];
          arr.contains(21).do({
                 co.out.println("Found");
          }).otherwise.do({
                 co.out.println("Not Found");
          });
