## Base and Backbone package

#### CO

   Yes it is CO and it is one of the ReservedWord

   This is the only package provided by default

   It contains following sub packages

   1.  lang

         Contains all the data types, kinds which folang support
        
           a. types
           b. kinds
 
    
   2.  sys

         contains all the utilities to handle 
        
           a. file
           b. concurrent 
           c. parallel
           d. goto
           e. invoke
           f. bind 
           g. call 
           h. apply 
           i. settimeout 
           j. setinterval
           k. schedular
           l. cron
           m. event

   3.  os
       
         OS Specific

           a. signal 
           b. cmd 
           c. execute  
           d. run  
           e. env 
           f. getenv
           g. setenv
           h. unsetenv
           i. sleep 
           j. exit
           k. cwd 
           l. chdir 
           m. fork
           n. wait 
           o. pipe
           p. dup
           q. close 
           r. readfd
           s. writefd

   4.  meta
       
         Meta is all about meta programming
         contains following

           a. patch      :  For patching exiting types, methods/functions, blocks etc
		   b. instrument :  Add observability/monitoring hooks
		   c. ast        :  Adding to AST mainly using macros of folang
		   d. reflect    :  Reflections reading metadata and allowing modification about anything
		   e. introspect :  Read only Reflection
		   f. transform  :  Run structural transformations over larger graphs
		   g. inject     :  Attach behavior or data from the outside
		   h. create     :  Creating new things
		   i. augment    :  Extend capabilities in a non-destructive way.
		   j. runtime    :  Which has eval the evil function like javascript evaluates any string (must be valid folang code ) at runtime without AST changes
   
   5.  core
        
        Contains all the collections like

           a. list
           b. set
           c. map
           d. tree
           e. tries
           f. sort
           g. search
            

   6.  native
       
         Native is all about low level control for programming
         
         Contains Following

           a. load 
           b. register 
           c. asm       : assembly code
           d. inline    : all about machine code
           e. emit      : all about actual instruction hex codes
           f. ffi
   
   
   7.  in
        
          This Package contains all the functionalities about input

             a. read
             b. readln
   
   8.  out
          
          This package contaiins all the functionalities about output

            a. println
            b. print
   
   9.  regex

         Package contains all the implmentations and functions ootb for regular expression handling  
            
            a. stex
            b. pattern
            c. match
            d. search

   
   10. crypto

         Package contains all the implementation and functions ootb for cryptography related libs

           1. rsa
           2. aes
           3. hash
           4. md5
           5. rand
           6. uuid
           7. ssl
           8. tls

   11. dap
   12. ddap

         Both 12 & 13 contains built in directives, decorators, annotations and pragmas
   
   13. net

         Package contains all the network related functionalities provided ootb
           
            1. tcp
            2. udp
            3. http
    
   14. const

         Constants 

            a. true
            b. false
            c. none

   15. encoding
  
         Package contains the followings

            a. base64Encode
            b. base64Decode
            c. json
            d. yml
            e. bson