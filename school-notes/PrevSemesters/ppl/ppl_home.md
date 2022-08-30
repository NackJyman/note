# Princibles of Programming Languages
## Homepage
---
Tutorials
- [[compiler | HW2 Implemtnation - Compiler]]

Class Stuff
- [[circuit_emulator | Circuit Emulator]]

# Uncatgorized Stuff (NEED TO DO)
##### Lecture 2 (Janurary 25th, 2022)
---

#### Lambda Calculus Grammar 
      ;``````````````;`````````````````````````;
      ;fn            ; These are all valid     ;
      ;\x -> fn y    ; programs to the parser. ;
      ;(\x -> x) fn  ;                         ;
      ``````````````````````````````````````````
---
#### Parse tree Example
When implementing a parse, when it comes to ambiguity. It is important to utilize precedence. How do we define subexpressions to build a tree like the example If allowed to occur anywhere, it highest form of precedence. parantheses and function applications also have their own grammar rules.

```ad-caution
Does not have the ASCII art example for Parse Tree see old notes.
```
---
### Grammar Rules
```ad-error
need to fix formatting here
```

```
〈lam〉 ::= ‘\’ ident ‘->’ 〈lam〉\\〈L0〉 ::= ‘\’ ident ‘->’ 〈L0〉
                | 〈app〉                //         | 〈L1〉
----------------------------------------||--------------------------------------
        〈app〉 ::= 〈app〉〈app〉        \\ 〈L1〉 ::= 〈L1〉〈L1〉
                | 〈var〉                //          |〈L2〉
----------------------------------------||--------------------------------------
        〈lam〉 ::= ident                \\ 〈L2〉 ::= ident
                |   '(' 〈lam〉 ')'      //          |   '(' 〈L0〉 ')'
----------------------------------------||--------------------------------------
```
  Ambiguity is bad when it leaves decisions to the parser. Avoided
by implementing precedence and associativity rules in the grammar.

---
#### End of Lecture 

---
---

##### Lecture 3 (1.27.2022)
## BNFC 
 BNFC - Backnus-Naur Form Converter. Converts BNF grammars into parsers.




---
###### LBNF
```
EInt. Exp ::= Integer ;
EMul. Exp ::= Exp "*" Exp ;
EAdd. Exp ::= Exp "+" Exp;
```

###### BNFC
```
〈exp〉 ::== integer
         |  〈exp〉'*' 〈exp〉
         |  〈exp〉'+' 〈exp〉
```

---
```
1. Feed the grammar file to BNFC
 - prints AbsExpr.hs
          PrintExpr.hs
          TestExpr.hs
---
jack@terminal$> bnfc Expr.cf
```
  Using the parsers:
      If we generate makefiles from BNFC, we get executable parsers by just
    typing 'make' after running BNFC, just by typing make. The test application
    accepts input from standard input, so we can test our grammar.

  On ambiguity:
      We need to implement precdence levels to clear up the order things are
    processed to ensure the interpreter correctly understands context. This can
    be accomplished using coercions.

---

#### Precedence levels in LBNF:

EAdd. Exp0 "+" Exp0 ;
_.    Exp0 ::= Exp1 ;

EMul. Exp1 ::= Exp1 "*" Exp1 ;
_.    Exp1 ::= Exp2 ;

EInt. Exp2 ::= Integer ;
_.    Exp2 ::= "(" Exp0 ")" ;
```
2. Generate our .info file to|___________________________/
Where we fix lack of   | jack@terminal$> happy ParExp.y
associativity.         |
```

  Generic ParExpr.info file layout Ex.
 
 ```
             _____________________________________________
            |State 16                                     |
            |    Expr1 -> Expr1 . '*' Exp1      (Rule 4)  |
            |    Expr1 -> Expr1 '*' Exp1        (Rule 4)  |
            |                                             |
            |    ')'                 Reduce using Rule 4  |
            |    '+'           Shift, and enter state 13  |
            |                      (reduce using rule 4)  |
            |                                             |
            |    %eof       reduce using rule 4           |
            |_____________________________________________|
```
  Closing Notes:
    - Look at Zoom video for more info
#### End of Lecture
---
---

##### Lecture 5 (Feburary 1st, 2022)

Discussing HW1
    - The best way to write the parser:
          m onad


##### Lectire 6		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  
		  