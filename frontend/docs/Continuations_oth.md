☑️ 1. Heap-based stack
Instead of pushing frames on the native stack, each call creates a heap frame.

Tail calls don’t increase call depth — they reuse or discard old frames.

☑️ 2. Trampolining
A "driver" loop (like a VM) runs functions that return "next functions to run".

Instead of returning normally, functions return what to do next — like CPS (Continuation-Passing Style).

☑️ 3. Continuation-Passing Style
Compilers rewrite code to pass the “what to do next” as an explicit argument.

Like:

lisp
Copy
Edit
(f x k)  ;; f gets input x, and continuation k
☑️ 4. Closures and Environments
To keep variable scope alive after jumping around, environments are boxed into heap-based structures.



Comparison with Other Implementations
Feature	Bigloo	Chez Scheme	Gambit
call/cc cost	Medium	Low	High
Stack model	Hybrid	CPS	Copying
Tail calls	Optimized	Optimized	Optimized
Multi-shot	Yes	Yes	Yes


Feature	Chicken Scheme	Bigloo Scheme
Implementation	Whole-program CPS	Stack copying
call/cc Cost	Low (heap allocation)	Medium (stack copy)
Memory Usage	Higher (all heap)	Lower (stack+heap)
Normal Execution	Some CPS overhead	Near-native speed
Multi-shot	Yes	Yes
Tail Calls	Optimized	Optimized
FFI Performance	Good	Excellent
GC Complexity	Higher (precise)	Lower (conservative)
Startup Time	Slower (whole-program)	Faster
Practical Implications


Feature	Gambit Scheme	Chicken Scheme
Implementation	Stack copying	Whole-program CPS
call/cc Cost	High (stack copy)	Low (heap alloc)
Memory Usage	Lower (stack segments)	Higher (all heap)
Normal Execution	Near-native speed	Some CPS overhead
Multi-shot	Yes	Yes
Tail Calls	Optimized	Optimized
FFI Performance	Excellent	Good
GC Precision	Conservative	Precise
Startup Time	Fast	Slower (whole-prog)


Gambit C++ Integration:
// In C++
class Processor {
public:
  virtual void process(int x) = 0;
};

// In Scheme
(define-c-lambda make-concrete-processor () Processor*
  "new ConcreteProcessor")
(define processor (make-concrete-processor))
(Processor-process processor 42)  // Direct virtual call

Chicken C++ Integration:

// Requires C wrapper layer
extern "C" {
  void* make_processor() { return new ConcreteProcessor; }
  void process(void* p, int x) { ((Processor*)p)->process(x); }
}

(define processor (make-processor))
(process processor 42)  // Indirect call



Noramal function call/ret
Delimited continuation = a portion of the program’s future execution, not everything like full call/cc.

You capture only part of the computation up to a delimiter (reset / shift or prompt / control) rather than the whole call stack.

More manageable, more modular, and more efficient than full continuations.


async/await



difference between below two

(define (outer)
  (display "Before reset\n")
  (reset
    (define (inner)
      (shift k
        (display "Inside shift\n")
        (k 'jump)))  ; Shift jumps to the continuation captured by the reset.
      (display "End inner\n")) ; This will never execute
  
    (inner)
    (display "After Inner\n"))  ; This won't be printed since control jumps out.

(outer)
(display "After outer"))  ; 





(define (outer)
  (display "Before callcc\n")
  (define (inner)
    (call/cc (lambda (k)
                (display "Inside call/cc\n")
               (k 'jump))) ; This jumps out of the call/cc
    (display "End inner\n")) ; This will never execute
  
  (inner)
  (display "After inner\n")) ; This will never execute because we jumped out

(outer)
(display "After outer\n");


RESET/SHIFT:

outer
 └── reset
      ├── display "Before reset"
      ├── inner
            ├── shift --> capture up to reset
            └── k('jump') --> jump to reset's boundary
      └── no "After Inner"
 └── "After outer" (YES)

CALL/CC:

outer
 ├── display "Before callcc"
 ├── inner
       ├── call/cc --> capture entire remaining program
       └── k('jump') --> jump to top-level
 └── no "After inner"
"After outer" (YES)





(define (callcc-example)
  (display "A\n")
  (call/cc (lambda (k)
    (display "B\n")
    (k 'jump)    ; Jumps to continuation (point where call/cc was called)
    (display "C\n"))) ; Never reached
  (display "D\n")) ; Never reached

(callcc-example)
(display "E\n")

[A] → [B] → [k invoked]
      ↑──────┘
      Skips everything else → [E]





(define (shift-reset-example)
  (display "A\n")
  (reset
    (display "B\n")
    (shift k
      (display "C\n")
      (k 'jump)   ; Jumps back to just after shift
      (display "D\n")) ; Never reached
    (display "E\n")) ; This still executes!
  (display "F\n"))

(shift-reset-example)
(display "G\n")
[A] → [reset: [B] → [shift: [C] → [k invoked]
                              ↑──────┘
                      Continues in reset: [E]] → [F] → [G]


boost ctx::callcc



4. Other "Higher" Concepts in Type Theory
(a) Higher-Order Logic
Extends first-order logic by allowing quantification over predicates (not just individuals).

Rarely used directly in programming languages but influences dependent types.

(b) Higher-Order Abstract Syntax (HOAS)
A meta-theoretic technique where binders in object languages are represented using functions in the meta-language.

Used in compilers/PL research (e.g., representing lambdas in ASTs).

(c) Higher-Inductive Types (HITs)
From homotopy type theory (HoTT), generalizes inductive types with path constructors.

Enables reasoning about equality up to homotopy.

(d) Higher-Order Modules
Modules (e.g., in ML) that take or return other modules.

Example (OCaml):




Comparison

Feature	Dynamic Typing	Duck Typing
Definition	Types are assigned to variables at runtime.	Type is determined by whether an object implements the necessary methods or behavior.
Type Determination	The type of a variable is determined at runtime based on the value assigned.	The "type" of an object is determined by its methods/behavior, not its class.
Focus	Focuses on the type of the variable (can change at runtime).	Focuses on whether an object can do what is required, based on behavior (methods/properties).
Type Checking	Type is checked at runtime.	No explicit type checking; behavior is what matters.
Common in	All dynamically typed languages (e.g., Python, Ruby, JavaScript).	Languages like Python and Ruby that follow dynamic typing.
Example	x = 5; x = "hello" in Python, type changes dynamically.	A dog object and a cat object both work as input to the same function as long as they implement a speak method.

Need C++ code for

The full continuation runtime code has been created with cumulative support for:
call/cc, reset/shift, prompt/control
deep Stack copying like Gambit (with auto stack-direction detection)
Multi-shot, resumable, reusable continuations
Structured return from reset/shift
Coroutine fallback using std::coroutine_handle
Serializable continuation frames
Chained composition of continuations
CHICKEN-style optional CPS emulation (via macro and coroutine interface)
CPS heap-based fallback mode, toggled via flag
More advanced coroutine/CPS integration




Yes, a GC becomes necessary if your language supports:

Cyclic references

Dynamic allocation-heavy idioms

Closures capturing long-lived environments

User-defined recursive data structures

Multi-shot continuations or coroutines

Algebraic effects with deep stacks of handlers

| Language Feature                             | Why GC helps                                                                 |
| -------------------------------------------- | ---------------------------------------------------------------------------- |
| ✅ **Closures with upward references**        | Captured variables may live longer than scope                                |
| ✅ **Lazy evaluation / thunks**               | Deferred computations can outlive creators                                   |
| ✅ **Multi-shot continuations / shift/reset** | The stack or heap may need to be preserved and copied                        |
| ✅ **Cyclic data**                            | Smart pointers can’t handle cycles without `weak_ptr`, which can get complex |
| ✅ **Dynamic effect handlers**                | Requires heap-allocated handlers and frames                                  |
| ✅ **REPL or dynamic environments**           | Hard to track static lifetimes                                               |
| ✅ **Language aims to be “safe”**             | GC avoids all manual memory bugs for end users                               |
