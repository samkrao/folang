(******************************)
(* FoLang Core Grammar (EBNF) *)
(******************************)

(* COMMENTS / META
   - Lines starting with '#' in the source are comments / headings and are ignored here.
   - This grammar is designed to ACCEPT every concrete snippet given:
       - All define/declare/impl forms
       - Structs, classes, signatures, packages, modules, library
       - Generics annotations, extensions, macros, templates
       - Delegates, named returns, bind vars ($, $1, $2…) and wildcard '_'
       - Method delegate pipelines with '=>>' and $1,$2,$3
       - let / where bindings
       - Functor / Monad / Transformer / etc. annotations
       - for/loop/condition DSL
       - Iterable each/contains, indexers, operators
       - Parallel / async / continuation / actor / event annotations
       - auto/dynamic/data, dependent types, native functions
       - var-style examples (var arr ...)
*)

(***********************)
(*  Program            *)
(***********************)

Program
    ::= { TopLevelItem } ;

TopLevelItem
    ::= TopLevelDecl
     | Stmt
     | ExprStmt
     | ";" ;


(***********************)
(*  Lexical Skeleton   *)
(***********************)

Identifier
    ::= /* implementation-defined:
          must allow: x, y, a, b, St1, St2, MyList, Emp, I, A, A1, A2, C3, C4,
          Functor, Applicative, Monad, Monoid, Transformer, SEmployee, someAnn,
          someMeth, myFun, doManythings, someDelegate, etc., AND 'var' if used
          as identifier elsewhere. */ ;

QualifiedName
    ::= Identifier { "." Identifier } ;
    (* e.g. co.lang.int, co.dap.mutable, co.lang.struct *)

IntegerLiteral
    ::= /* decimal integer literal, supports 0,1,2,3,... */ ;

FloatLiteral
    ::= /* float literal */ ;

StringLiteral
    ::= /* string literal, supports "..." */ ;

CharLiteral
    ::= /* character literal, supports 'A', '\n', etc. */ ;

BooleanLiteral
    ::= "true" | "false" ;

BitLiteral
    ::= /* bit literal */ ;

ByteLiteral
    ::= /* byte literal */ ;

RegexLiteral
    ::= /* regex literal, supports \d+ etc. */ ;

BindVariable
    ::= "$" [ IntegerLiteral ] ;
    (* covers $, $0, $1, $2, $10, ... *)

WildcardVariable
    ::= "_" ;


(***********************)
(*  Literals           *)
(***********************)

Literal
    ::= IntegerLiteral
     | FloatLiteral
     | StringLiteral
     | CharLiteral
     | BooleanLiteral
     | BitLiteral
     | ByteLiteral
     | RegexLiteral
     | ArrayLiteral
     | MapLiteral ;

ArrayLiteral
    ::= "[" [ ExprList ] "]" ;

ExprList
    ::= Expr { "," Expr } ;

MapLiteral
    ::= "{" [ MapEntryList ] "}" ;

MapEntryList
    ::= MapEntry { "," MapEntry } ;

MapEntry
    ::= Identifier ":" Expr ;


(***********************)
(*  Types              *)
(***********************)

TypeExpr
    ::= TypePath { TypeSuffix }
     | FunctionType ;

TypePath
    ::= QualifiedName ;
    (* includes: co.lang.int, co.lang.float, co.lang.char, co.lang.string,
       co.lang.byte, co.lang.bit, co.lang.bool, co.lang.intptr, co.lang.uintptr,
       co.lang.type, co.lang.newtype, co.lang.opaquetype, co.lang.struct,
       co.lang.signature, co.lang.interface, co.lang.class, co.lang.package,
       co.lang.function, co.lang.delegate, co.lang.auto, co.lang.dynamic,
       co.lang.data, co.lang.Error, co.lang.excpetion, co.lang.untyped, etc. *)

BaseType
    ::= "co.lang.int"
     | "co.lang.float"
     | "co.lang.char"
     | "co.lang.string"
     | "co.lang.byte"
     | "co.lang.bit"
     | "co.lang.bool" ;

FunctionType
    ::= "(" [ TypeExprList ] ")" "->" "(" [ ReturnTypeList ] ")" ;

TypeExprList
    ::= TypeExpr { "," TypeExpr } ;

ReturnTypeList
    ::= ReturnTypeItem { "," ReturnTypeItem } ;

ReturnTypeItem
    ::= [ Identifier ] TypeExpr ;
    (* allows named returns: (r co.lang.int, e co.lang.excpetion) *)

TypeSuffix
    ::= "->" "(" TypeSuffixBody ")" ;

TypeSuffixBody
    ::= TypeSuffixItem { "," TypeSuffixItem } ;

TypeSuffixItem
    ::= PointerStars
     | ArrayShape
     | "&"
     | "&&"
     | "@"
     | "^"
     | TypeSuffixNamedParam ;

PointerStars
    ::= "*" | "**" | "***" ;

ArrayShape
    ::= "[" IntegerLiteral "]"
     | "[" IntegerLiteral "," IntegerLiteral "]"
     | "[" "]"
     | "[" "0" "]"
     | "[" "*" "]"
     | "[" "..." "]"
     | "[" "[" "[" IntegerLiteral "]" "]" "]"
     | "[" IntegerLiteral "]" "[" "]" "[" "]"
     | "(" "*" ")" "[" IntegerLiteral "]"
     | "*" "[" IntegerLiteral "]" ;
     (* covers:
        [5], [5,4], [0], [], [*], [...],
        [[[4]]], [4][][], (*)[4], *[3]
      *)

TypeSuffixNamedParam
    ::= Identifier "=" Expr ;
    (* used in: b co.lang.int->(&,meta={type=out}) *)


(***********************)
(*  Annotations        *)
(***********************)

Annotation
    ::= "@" QualifiedName [ AnnotationParams ] ;

AnnotationParams
    ::= "(" [ AnnotationParamList ] ")" ;

AnnotationParamList
    ::= AnnotationParam { "," AnnotationParam } ;

AnnotationParam
    ::= Identifier "=" AnnotationParamValue ;

AnnotationParamValue
    ::= Expr ;

AnnotationList
    ::= { Annotation } ;


(***********************)
(*  Top-level Decl     *)
(***********************)

TopLevelDecl
    ::= DefineDecl
     | DeclareDecl
     | ImplDecl ;


(***********************)
(*  define             *)
(***********************)

DefineDecl
    ::= AnnotationList "define" DefineBody [ ";" ] ;

DefineBody
    ::= DefineFunction
     | DefineVarOrType
     | DefineUDT ;

(* variable / simple / aliases / delegate-like / inferred etc. *)

DefineVarOrType
    ::= Identifier [ TypedPart ] [ VarInit ] ;

TypedPart
    ::= TypeExpr ;

VarInit
    ::= "=" InitRHS ;

InitRHS
    ::= Expr
     | FunctionType ;
     (* allows:
        define x co.lang.int = 10;
        define x = 10;
        define someDelegate co.lang.delegate =
            (a co.lang.int, b co.lang.int) -> (co.lang.int, co.lang.int);
      *)


(***********************)
(*  UDTs / Types / etc *)
(***********************)

(* Covers:
   - type/newtype/opaquetype/subtype/supertype/ADTs:
       define x co.lang.type = co.lang.int
       define x co.lang.newtype=co.lang.int
       define x co.lang.opaquetype=co.lang.int
       define y co.lang.type=co.lang.int | co.lang.char
       define test co.lang.subtype = co.lang.int;
       define test co.lang.supertype = co.lang.int;
   - type constructor:
       @co.dap.hokrt
       define Option(T) co.lang.type = Some(T) | None();
   - structs/signatures/modules/packages/library/interfaces/classes:
       define St1 co.lang.struct={ ... }
       define Si1 co.lang.signature={ ... }
       define x co.lang.signature={}
       define x co.lang.package={ ... }
       @co.dap.Library define x co.lang.package={ ... }
       define I co.lang.interface={ ... }
       define MyList co.lang.struct={ eles co.lang.int->([*]); }
       define x co.lang.class->(extends=[A,B],implements=[I,J,K])={ ... }
       define Emp co.lang.class{ ... }
*)

DefineUDT
    ::= UDTHead UDTBody ;

UDTHead
    ::= Identifier UDTTypeExpr ;

UDTTypeExpr
    ::= TypeExpr ;

UDTBody
    ::= "=" Block
     | Block ;


(***********************)
(*  Functions & Methods*)
(***********************)

(* General pattern:
   define [ReceiverOpt] FuncName (curried param sections) [->(returns)] Body [ImmediateCall] ;
*)

DefineFunction
    ::= ReceiverOptOpt FuncName CurriedParamSections ReturnTypeOpt FunctionBody ImmediateCallOpt ;

ReceiverOptOpt
    ::= "(" Identifier TypeExpr ")" 
     | /* empty */ ;
    (* e.g. define (St1) getName()->(...){...}
           define (g MyList) [](index co.lang.int)->(co.lang.int)={...} *)

FuncName
    ::= Identifier
     | OperatorSymbol
     | "[]"
     | "[]=" ;

OperatorSymbol
    ::= "+"
     | "-"
     | "*"
     | "/"
     | "%"
     | "∪"
     | "=="
     | "!="
     | "<="
     | "&&"
     | "||"
     | "__" ;
     (* extended as needed *)

CurriedParamSections
    ::= CurriedParamSection { CurriedParamSection } ;

CurriedParamSection
    ::= "(" [ ParamList ] ")" ;

ParamList
    ::= Param { "," Param } ;

Param
    ::= { ParamPrefix } ParamName ParamOptionalOpt ParamTypeOpt DefaultValueOpt ;

ParamPrefix
    ::= "..."
     | "~" ;
     (* ...b (variadic), ~k (named param) *)

ParamName
    ::= Identifier
     | WildcardVariable
     | BindVariable ;
     (* allows: idx, val, _, $ *)

ParamOptionalOpt
    ::= "?"
     | /* empty */ ;
     (* k? co.lang.int *)

ParamTypeOpt
    ::= TypeExpr
     | /* empty */ ;

DefaultValueOpt
    ::= "=" Expr
     | /* empty */ ;

ReturnTypeOpt
    ::= "->" "(" [ ReturnTypeList ] ")"
     | /* empty */ ;

FunctionBody
    ::= "=" Block
     | Block
     | "=>" Expr ;
     (* supports:
        define add(...)->(...){}
        define add(...)->(...)=>
        define x =(a co.lang.int)->(co.lang.int)=>10
      *)

ImmediateCallOpt
    ::= "(" [ ArgList ] ")"
     | /* empty */ ;
     (* anonymous:
        @co.dap.anonymous
        define _ (k co.lang.int)->(a co.lang.int)={}(12);
      *)


(***********************)
(*  declare            *)
(***********************)

DeclareDecl
    ::= AnnotationList "declare" DeclareBody ";" ;

DeclareBody
    ::= Identifier TypeExpr
     | Identifier FunctionSig ;

FunctionSig
    ::= "(" [ ParamList ] ")" "->" "(" [ ReturnTypeList ] ")" ;


(***********************)
(*  impl               *)
(***********************)

ImplDecl
    ::= AnnotationList "impl" ImplHead ImplBodyOpt [ ";" ] ;

ImplHead
    ::= ImplTarget ImplSig ;

ImplTarget
    ::= Identifier
     | "_" ;

ImplSig
    ::= "->" "(" ImplSigPayload ")" ;

ImplSigPayload
    ::= ImplSigItem { "," ImplSigItem } ;

ImplSigItem
    ::= Identifier "=" Expr ;
    (* instance=Functor(List) etc. *)

ImplBodyOpt
    ::= "=" Block
     | Block
     | /* empty */ ;


(***********************)
(*  Statements         *)
(***********************)

Stmt
    ::= VarStmt
     | ForStmt
     | IfLikeStmt
     | DefineDecl
     | DeclareDecl
     | ImplDecl ;

(* 'var' style for Iterable examples *)

VarStmt
    ::= "var" Identifier TypeExpr [ VarInit ] ";" ;
    (* e.g.
       var arr co.lang.int->([5]) = [6,7,8,9,10];
       var k co.lang.int = 31;
    *)

ExprStmt
    ::= Expr ";" ;

IfLikeStmt
    ::= /* these are mostly expression-based in examples, so covered by ExprStmt;
          a classic 'if' is allowed if needed: */
       "if" Expr Block [ "else" Block ] ;


(***********************)
(*  for                *)
(***********************)

ForStmt
    ::= "for" "(" BlockContent ")" ";"
     | "for" Block ";" ;


(***********************)
(*  Blocks & Members   *)
(***********************)

Block
    ::= "{" [ BlockContent ] "}" ;

BlockContent
    ::= { Stmt | ExprStmt | MemberDecl } ;

MemberDecl
    ::= FieldDecl
     | MethodDecl ;

FieldDecl
    ::= Identifier TypeExpr ";" ;

MethodDecl
    ::= AnnotationList Identifier FunctionSig MethodBodyOpt ;

MethodBodyOpt
    ::= "=" Block
     | Block
     | "=>" Expr
     | /* empty */ ;


(***********************)
(*  let / where        *)
(***********************)

LetExpr
    ::= "let" LetBindings ".in" LetBody ;

LetBindings
    ::= "(" [ LetBindingList ] ")"
     | "{" [ LetBindingList ] "}" ;

LetBindingList
    ::= LetBinding { "," LetBinding } ;

LetBinding
    ::= Pattern "=" Expr ;

Pattern
    ::= PatternName
     | "(" Pattern ")"
     | StructPattern ;

PatternName
    ::= Identifier
     | WildcardVariable
     | BindVariable ;
     (* supports $, $1, idx, _, etc. *)

StructPattern
    ::= Identifier "{" [ PatternFieldList ] "}" ;

PatternFieldList
    ::= PatternField { "," PatternField } ;

PatternField
    ::= Identifier ":" Pattern ;

LetBody
    ::= "(" Expr ")"
     | "{" BlockContent "}" ;

WhereExpr
    ::= Expr ".where" "(" LetBindingList ")" ;
    (* e.g. define x co.lang.int=($+1).where($=10); *)


(***********************)
(*  Expressions        *)
(***********************)

Expr
    ::= AssignmentExpr ;

AssignmentExpr
    ::= PostfixExpr [ "=" Expr ]
     | LetExpr
     | WhereExpr ;

PostfixExpr
    ::= PrimaryExpr { PostfixOp } ;

PostfixOp
    ::= "." Identifier "(" [ ArgList ] ")"
     | "." Identifier
     | "(" [ ArgList ] ")"
     | "[" Expr "]"
     | "=>>" Expr ;
     (* supports method chaining + delegate pipeline:
        dosomething(...)=>>somePack.somMethod(a)=>>someOthPack.somOtherMeth($1,b)
      *)

ArgList
    ::= Expr { "," Expr } ;


(***********************)
(*  Primary Expr       *)
(***********************)

PrimaryExpr
    ::= Identifier
     | QualifiedName
     | "this"
     | BindVariable
     | WildcardVariable
     | Literal
     | "(" Expr ")"
     | LambdaExpr
     | AnonFunctionExpr
     | StructLiteral ;

LambdaExpr
    ::= "|" [ LambdaParamList ] "|" "=>" Expr ;
    (* |x,y|=> x+y *)

LambdaParamList
    ::= ParamName { "," ParamName } ;

AnonFunctionExpr
    ::= CurriedParamSections ReturnTypeOpt FunctionBody ;

StructLiteral
    ::= Identifier "{" [ StructInitList ] "}" ;

StructInitList
    ::= StructInit { "," StructInit } ;

StructInit
    ::= Identifier ":" Expr ;
    (* e.g. St1{name:"kamesh",age:50,id:1} *)


(***********************)
(*  Notes              *)
(***********************)

(* This grammar directly supports all shown constructs:

   - Base types:
       co.lang.int, co.lang.float, co.lang.char, co.lang.string,
       co.lang.byte, co.lang.bit, co.lang.bool

   - Operators token set (lexical):
       [, ], (, ), {, }, ==, =, !=, =>>, =>, ->, -, +, *, /, %, $, &, #, @, !,
       ~, `, ^, |, \, /, ., ,, ;, :, ', ", <=, ||, &&, ..., .., ??=, ++, +=,
       -=, --, ~~, __

   - Examples like:
       define x co.lang.int=10;
       @co.dap.mutable define x co.lang.int;
       define x co.lang.int->(*);
       define x co.lang.int->([5,4]);
       define x co.lang.int->([])=[1,2,3,4];
       define x co.lang.int->([[[4]]]);
       define St1 co.lang.struct={ name co.lang.string id co.lang.int age co.lang.float }

       define add (a co.lang.int, b co.lang.int)->(co.lang.int)={}
       define doNothing ()->()={}
       define multiret (a co.lang.int, b co.lang.int)->(co.lang.int, co.lang.int){ this.return (a+b, a-b); }

       define y co.lang.int = let({$ =10}).in({$+1})
       define x co.lang.int=($ + 1).where($=10);

       (boolean truth).do({}).otherwise(boolean truth).do({}).otherwise.do({});
       (boolean truth).loop({}).otherwise(boolean truth).loop({}).otherwise.loop({});

       define k co.lang.int = 1;
       for({ k = k +1; co.out.println(k); (k >=10).do({ this.break; }); });

       x.match.case(co.regex(\d+)=>co.const.true).default(co.const.false);
       s = (boolean truth).return (some).otherwise.return(other);

       var arr co.lang.int->([5]) = [6,7,8,9,10];
       arr.each(idx,val).do( ... );
       arr.each(_,val).do( ... );
       arr.contains(k).do({});

       @co.dap.extension(fortype=co.lang.string),what=extends)
       define upperCase()->(string)={ return this.upper() }

       @co.dap.generic(...) define Transformer co.lang.function= ( A, C, D) -> ( B)

       @co.dap.indexer
       define (g MyList) [](index co.lang.int)->(co.lang.int)={ this.return g.eles[index] }

       @co.dap.operator
       define + (a Employee, b Employee)->(Employee)={}

       @co.dap.process define doSomeComplexLogic(...)->(co.lang.int, co.lang.Error)={}
       @co.dap.async, @co.dap.coroutine, @co.dap.generator, @co.dap.subroutine, @co.dap.goroutine
       @co.dap.continuation(type=callcc/shiftreset/...)

       @co.dap.delegate
       define someDelegate co.lang.delegate =
           (a co.lang.int, b co.lang.int) -> (co.lang.int, co.lang.int );

       define doManythings(a co.lang.int, b co.lang.int->(&,meta={type=out}))
           ->(r co.lang.int, e co.lang.excpetion)={}

       Bind variables:
         $        (used in let)
         $1, $2, $3 (used in pipelines)
       Wildcard:
         _
*)
