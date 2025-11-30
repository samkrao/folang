### Library

    1. Library Declaration

        Libraries are standalone modules in folang that may contain utilities, and implementations can be re used, similar to static libraries in C++ or jars in java
        
        a. Structs and Free functions

            @co.dap.library
            define EmpPackage co.lang.package={
                @co.dap.export
                define SEmployee co.lang.signature={
                    declare Employee co.lang.struct;
                    declare storeEmployee ( emp Employee)->(Employee);
        
                }

                define Employee co.lang.struct={
                    empId co.lang.int;
                    empName co.lang.string;
                }

                define storeEmployee(emp Employee)->(Employee)={
                    define e = Employee();
                    this.return e;
                }

            }
        
    
        2. Classes 

            @co.dap.library
            define EmpPackage co.lang.package={
                @co.dap.export
                define IEmployee co.lang.interface={

                    declare storeEmployee ( emp Employee)->(Employee);
            
                }

                @co.dap.oops(
                    Implements:[IEmployee],
                )
                define Employee co.lang.class->(implements=[IEmployee])={
                    empId co.lang.int;
                    empName co.lang.string;
                
                    storeEmployee(emp Employee)->(Employee)={
                        e = Employee();
                        this.return e;
                    }

                }

            }
        3. Modules

            @co.dap.library
            define EmpPackage co.lang.package={
                @co.dap.export
                define MEmployee co.lang.signature={
                    declare Employee struct;
                    declare storeEmployee ( emp Employee)->(Employee);
            
                }
                @co.dap.modulesig(MEmployee)           )
                impl EmployeeImpl co.lang.module->(matches=MEmployee)={
                define Employee co.lang.struct = {
                        empId   co.lang.int;
                        empName co.lang.string;
                    }
                
                    define storeEmployee(emp Employee)->(Employee)={
                        define e = Employee();
                        this.return e;
                    }

            }





