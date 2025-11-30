### Languagge

#### Data Types

##### Base Types

    1. co.lang.int
    2. co.lang.long
    3. co.lang.float
    4. co.lang.bit
    5. co.lang.char
    6. co.lang.bool
    7. co.lang.string

###### UDT

    1. co.lang.struct (pure type no initialization no methods)
    2. co.lang.class  (classes)
       
        a. co.dap.abstract
        b. co.dap.virtual
        c. co.dap.interface
        d. co.dap.trait
        e. co.dap.mixin

    3. co.lang.module (Module signatures)
    4. co.lang.object (objects)
    5. co.lang.function (functions)
       
       a. co.dap.annotation
       b. co.dap.directive
       c. co.dap.macro
       d. co.dap.pragma
       e. co.dap.decorator
       f. co.dap.template
       g. co.dap.generic
       h. co.dap.parametric
       i. co.dap.lambda
       j. co.dapp.anonymous
       k. co.dap.indexer
       l. co.dap.extension

    6. co.lang.package
    7. co.lang.enum

##### Special qualifiers

    1. co.dap.public
    2. co.dap.private
    3. co.dap.internal
    4. co.dap.package
    5. co.dap.protected
    6. co.dap.volatile
    7. co.dap.const
    8. co.dap.final

##### special types

   1. Pointers ( \*,\*\*,\*\*\*)

      co.lang.pointer

   2. arrays ([],[,], [...],\]\[,[0])
      
        co.lang.array

          a. jagged
          b. multi dimensional
          c. FAM/VLA/dynamic
          d. Zero Dimension
   
   3. References (&, &&)

        co.lang.ref
        
           i co.dap.lref
          ii co.dap.rref

           a. LValue
           b. RValue

   4. Addresses (@)

          a. co.lang.addr
          b. co.lang.ptr
          c. co.lang.uptr
          d. co.lang.intptr
          e. co.lang.floatptr 
   
   5. Thunks (^)

#### Built in methods/stament/expressions

   Methods

       1. co.out.println
       2. co.out.print
       3. co.eval
       4. co.invoke
       5. co.apply
       6. co.bind
       7. co.execute
       8. co.call
       9. co.command
      10. do
      11. otherwise
      12. loop
      13. match
      14. case
      15. return
      16. contains
      17. each
      18. co.introspect
      19. co.reflect
      20. when
      21. with
      22. inject

   Statments
      
      1. this.break
      2. this.return
      3. this.prototype
      4. co.nop
      5. this.continue
      6. this.class
      7. this.type
      8. this.object
      9. this.instance

   Expressions

      1. co.expr
      2. co.stmt