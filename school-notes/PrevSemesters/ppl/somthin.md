# debug session
#### Feburary 22, 2022
###### HW 3
---
Look at the documentation:
https://bnfc.readthedocs.io/en/latest/lbnf.html
https://bnfc.readthedocs.io/en/latest/lbnf.html#lexer-definitions


```cf
Prog. Program ::= [Def]

FunDef.       Def ::= Type ID "(" [Arg] ")" FunBody ;
InlineFunDef. Def ::= "inline" Type Id "(" [Arg] ")" FunBody ;

FBlk.   FunBody ::= "{" [Stm] "}"
FEmpty. FunBody ::= ";" ;

seperator Arg "," ;

TyArg. Arg ::= Type ;

TQual. Type ::= QualC;
TLit.  Type ::= LitType;

rules LitType ::= "int" | "double" | "bool" | "void"

Qual. QualC ::= [Name] ;
NId.  Name ::= Id ;
NTpl. Name ::= Id "<" [Type] ">" ;
seperator nonempty Name "::" ;

token Id (letter (letter | digit | '_')*);
```

