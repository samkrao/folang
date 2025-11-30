#### Indexers

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

###### Example usage

     a. 
     
         define lst MyList->([10]);
         define k co.lang.int = lst[2];

     b.

        lst[3] = 22;
