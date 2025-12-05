# Exception And/Or Errors And Effects

### Functions and Blocks can throw Exceptions

#### In co Lang Blocks are not silos they should be part of functions which narrow downs to Exception handling at Functions

#### In co Lang Programmer can attach which function to be executed in the event of exception

#### It is similar to Aspects for that matter Co Lang gives following 

     1. Before         (@co.ddap.before)
     2. After          (@co.ddap.after)
     3. Around         (@co.ddap.around)
     4. defer          (@co.ddap.defer)
     5. OnError        (@co.fx.onErrExcept)  
     6. Always         (@co.fx.InovkeAlways)
     7. HandleEffect   (@co.fx.HandleEffect)
     

    To the above Programmer can assign functions to handle these functions all the variables declared with (@co.dap.lexicalscope) will be available to these functions 

#### registering callbacks in the function itself.

     1. this.onPanic.do({});
     2. this.always.do({});
     3. this.ensure.do({});
     4. this.defer.do({});