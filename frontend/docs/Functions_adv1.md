### Functions of other types

    These functions 
        
        1. Should not contain any other code other than co.asm or co.mi
        2. These function cannot be passed or returned
        3. These functions cannot be inner/anonymous or closures/curried 
        4. These cannot be used as methods of class
        5. These cannot be generic/parametric
        6. These cannot be overloaded 

    1.  Assembly language
         @co.dap.native   
         define  add (a co.lang.int, b co.lang.int)->(co.lang.int)={
            var result co.lang.int;
            this.return co.asm{
                mov eax, a
                add eax, b
                mov result, eax
            }

         }
    
    2. Machine code
        @co.dap.native
        define add (a co.lang.int, b co.lang.int)->(co.lang.int)={
            var result co.lang.int;
            this.return co.mi{
                ".byte 0x8b, 0xc7;" // mov eax, edi (on System V ABI, edi = a)
                ".byte 0x01, 0xf7;" // add edi, esi (esi = b)
                : "=a" (result)
                : "D" (a), "S" (b)
            }

         } 