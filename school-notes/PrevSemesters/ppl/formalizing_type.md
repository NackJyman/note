# Formalizing Type Format
###### Principals of Programming Languages

---

### Defining _Inference Rule_ Notation

##### Typing Relation for Expressions

$$\Gamma \vdash_e e : T$$

This logical notation is called a _Judgement._ Which gives use informations of type e.

Our typing relation needed to include some expressions that contained other expressions. In other wordsm we watned the following to be tru:

$$x : int, \ y : int \ \vdash_e e \le y : bool$$
We used _inference rule_ notation to represent these cases. For example the dollowing rule that adds well-typed less-than-or-equal-to expressions to the typing relation:

$$\frac{\Gamma \vdash_e e_1 : int \ \ \ \ \ \ \ \ \Gamma \vdash_e e_2 : int}{\Gamma \vdash_e e_1 \le e_2 : bool}$$

_Inference rules_ can be read similarly to logical implications. The above says the expression "$e_1 \le e_2$" has type _bool_ in environment $\Gamma$ if and only if $e_1$ and $e_2$ noth have type int in environment $\Gamma$.

- The top section of the _inference rule_. Describe the conditions in which the bottom section are true. IE: Within the environment $\Gamma$, if $e_1$ and $e_2$ are of type '$int$' then the expression $e_1$ and $e_2$ are type '$bool$'. 

---

#### Typing Relations for Statements

###### Example Grammar File _(*.cf)_
```hs
PImp. Prog ::= [Stm];

SInit.  Stm ::= Ident ":=" Exp ";" ;
SAss.   Stm ::= Ident "=" Exp ";" ;
SBlock. Stm ::= "{" [Stm] "}" ;
SIf.    Stm ::= "if" Exp "then" Stm "else" Stm ;
SWhile. Stm ::= "while" Exp "do" Stm ;
SPrint. Stm ::= "print" Exp ";" ;

seperator Stm "" ;

EOr.  Exp  ::= Exp1 "||" Exp  ;
EAnd. Exp  ::= Exp1 "&&" Exp  ;
EEq.  Exp1 ::= Exp1 "==" Exp2 ;
ELeq. Exp1 ::= Exp1 "<=" Exp2 ;
ENot. Exp2 ::=      "!"  Exp4 ;
ESub. Exp2 ::= Exp2 "-"  Exp3 ;
EAdd. Exp2 ::= Exp2 "+"  Exp3 ;
EMul. Exp3 ::= Exp3 "*"  Exp4 ;
ETru. Exp4 ::= "true"         ;
EFls. Exp4 ::= "false"        ;
EVar. Exp4 ::= Ident          ;
EInt. Exp4 ::= Integer        ;

coersions Exp 4 ;

comment "--" ''
```

Let's look step by step what the type checker does when it type checks the statement

```cpp
if true || false then print 1; else print 2
```

We would check the expression after "$if$" and see that it has a type "$bool$". After this would also want to check the "print" function calls.

##### JavaScript Type Checking Function Example
This function simply checks the types within a statement and recursively checks the pieces/expressions of the statement.
```js 
@Override
public Void visit(SIf p, Void arg) {
	ImpType expType = p.exp_.accept(this, arg);
	if (expType.equals(ImpType.T_BOOL)) {
		p.stm_1.accept(this, arg);
		p.stm_2.accept(this, arg);
		return null;
	}
	throw new ImpTypeError("...");
}
```

Remember that this function does not change the environment. 

So we need to write our typing relations for statements to somehow restrict what types of expressions can occur where, if necessary. **This makes the relation for statements depend on the relation for expressions**! 

Also notice that we do not get an explicit result from the visit methods for statements.
##### JavaScript checking the statement "$print(1)$"
This check definition merely checks to make sure that the expression within the print has a type. It does not care what it is.

```js 
@Override
public Void visit(SPrint p, Void arg) {
	p.exp_.accept(this, arg);
	return null;
}
```


We're seeing here how type checking relies on type ingerence. Whenever an expression occurs in a statement, we infer its type and then check that the type fits the place it occurs in the statement. We've already seen this in the way we type check expressions.

##### JavaScript checking the statement '$x := 5$'

```js
@Override
public Void visit(SInit p, Void arg) {
	environment.get(0).put(p.ident_, p.exp_.accept(this, arg));
	return null;
}
```

This sets the type of x to the type of the expression in the local level of the environment! This hints at how our simple language is handling variable scopes, and it means we need to start building "scoping" into our environments in our specification.

It also means we need to somehow represent the fact that statements may update the environment.

- In this implementation we allow for the possiblility of a variable to be reinitalized. 
- An important element of this concept is the fact that some statements will need to make alterations to the environment.

##### Statement Typing Relation Summary
- Must only allow certain types of expressions in certain places, any type of expression in others.
- Must depend on the expression typing relation.
- Must domehow represent the updating of environments.

---

The ststement typing relation is similar to the expression typing relation except it doesn't relate epressions and environments to types, it relates statements and environments to _new environments_!

With that, we have descovered that the typing relation for statements is a 3-place relation of environments, statements, and environment resulting from executing that statement in that environment.

If a triple is in the relation, which we'll denote as $\vdash_s$, we write:

$$\Gamma \vdash_s stm \Rightarrow \Gamma'$$

Where $\Gamma$ is the environment (just a map from identifier to type), _stm_ is the expression, and $\Gamma'$ is the environment resulting from running _stm_ in $\Gamma$.

###### Running a statement will result in a new environment

### Variable Scoping, Environments and Context

#### New notation describing the "scope" of variables.
We now need to introduce some new notation that will allow us to talk about the scopes of variables in the environment. So let's think about how we might want to do this.

In general, every variable binding exists in the scope (block) it occurs in and every block that occurs after that binding until it is redeclared. 

Variables are "shadowed" if a binding occurs in a scope benath the scope the original binding occured in.

#### Example Statements
Here are some example statements, annotated with the environment that exists at each step:

```hs
x := 5;          -- [[(x,int)]]
if true then {   -- [[],[(x,int)]]
	x = 10;        -- [[],[(x,int)]]
	y := true;     -- [[(y,bool)],[(x,int)]]
	x := false;    -- [[(y,bool), (x,bool)],[(x,int)]]
} else {         -- [[],[(x,int)]]
	x := true;     -- [[(x,bool)],[(x,int)]]
	y := 4;        -- [[(x,bool),(y,int)],[(x,int)]]
}                -- [[(x,int)]]
y := 10;         -- [[(x,int),(y, int)]]
```

Shadowing occurs at the third statement of the if block and the first statement of the else block.

- Breaking environment (shown by the comments in the code block above) is the list of decalaration.
- The global context is that first 'x:=5'
- When we enter into the if-statement we have a localized environment.
- When these variables are initialized, we place these variables into the localized environment. This same behavior is shown within the else statement.
- Once the code block ends, so does the existance of these variables in the local environment.
- Notice that the 'y:=10' statement at the end of the code block is initialized in the global environment.

Obviously, if we encounter a variable during type checking, we need to be able to look up its type. And its type comes from the "closest" binding in the environment.

So environments consist of a list-like structure of maps from identifiers to types, where each map is preceded by inner scopes, and succeeded by outer scopes. We refer to the maps as _contexts,_ which consist of the local bindings (maps) at each scope (block).

There's many ways to implement this, but the simplest is as lists, where the first element is the most nested context, and the last element is the global context.

- Environments is a list of varying levels of importance. The last element in the list is a tuple of all the global variables (_identifier, type_). And the elements in front are local variables for code blocks.
- Scope is a code block (In more complicated languages, it would also include function code blocks.)

Implementation within the type checker:
```js
...
private List<Hashmap<String, ImpType>> environment;
...
```

Using lists makes the variable lookup and declaration more straightforward as we will see.


#### New Notation, New Me
We will still write environments as $\Gamma$ whenever we refer to an arbitrary environment. If we need to refer to indicidual contexts, we will also write them as $\Gamma$, but seperated with the $\rhd$ character like so:

$$\Gamma \rhd \Gamma'$$

In this example, $\Gamma'$ is the "lowest" context.

Since our typing relation for expressions doesn't much rely on variable scoping, the typing rules stay the same. For example, the typing tule for variable expressions:

$$\frac{id : T \in \Gamma}{\Gamma \vdash_e id : T}$$

So we're abusing set membership notation a little bit here, and saying that $id: T \in \Gamma$ means the closest binding of _id_ is $\Gamma$ in $T$.

For statements, the scope of variables does start to matter. For example, the rules for type checking variable initializations and assignments:

$$\frac{\Gamma \vdash_e e : T}{\Gamma \vdash_s id := e \Rightarrow \Gamma, id:T} \ \ \ \ \ \ \ \ \ \frac{\Gamma \vdash_e e : T \ \ \ \ \ \ \ id : T \in \Gamma}{\Gamma \vdash_s id = e \Rightarrow \Gamma}$$

The meta-expression $\Gamma, id : T$ means to map _id_ to type $T$ in the local context of environment $\Gamma$.

- These are generaric extractions of mapping the _identiy_ ($id$) to a specified _type_ ($T$) within the environment $\Gamma$.
- "$\Gamma \vdash_e e : T$" is stating that expression $e$ within $\Gamma$ needs to have some type.

###### The rule vs. the implementation for initialization:

$$\frac{\Gamma \vdash_e e : T}{\Gamma \vdash_s id := e \Rightarrow \Gamma, id : T}$$

```js
@Override
public Void visit(SInit p, Void arg) {
	environment.get(0).put(p.ident_, p.exp_.accept(this, arg));
	return null;
}
```
_As seen above, Type Checking "x:=5"_

###### The rule vs. the implementation for assignment:

$$\frac{\Gamma \vdash_e e : T \ \ \ \ \ \ \ \ id:T \in \Gamma}{\Gamma \vdash_s id = e \Rightarrow \Gamma}$$


```js
@Override
public Void visit(SAss p, Void arg) {
	ImpType expType = p.exp_.accept(this, arg);
	for (HashMap<String, ImpType> ctx : environment) { 
		if (ctx.containsKey(p.ident_)) {
			if(ctx.get(p.ident_).equals(expType))
				ctx.put(p.ident_, p.exp_.accept(this, arg));
			else
				throw new ImpTypeError("Type Mismatch");
			return null;
		}
	}
	throw new ImpTypeError("Variable not initialized");
}
```

###### Typechecking a Block Statement

```js
@Override
public Void visit(SBlock p, Void arg) {
	environment.add(0, new HashMap<>());
	for (Stm stm : p.liststm_) {
		stm.accept(this, arg);
	}
	environment.remove(0);
	return null;
}
```

- Go through each statement within the code block and then remove them all once each is checked.

A block statement is only well-typed in an environment $\Gamma$ if it's list of sub-statements is well-typed in $\Gamma \rhd \varnothing$, where $\varnothing$ is the empty context.

$$\frac{\Gamma \rhd \varnothing \vdash_s s_0 ...s_n \Rightarrow \Gamma'}{\Gamma \vdash_s \{ s_0 ...s_n\} \Rightarrow \Gamma'}$$

- The '$s$' values here are substatements within the code block.

However, with the rules we have so far, we dont know how to type check lists of statements! We do this by showing how to type check non-empty and empty lists of statements:

$$\frac{\Gamma \vdash_s s_0 \Rightarrow \Gamma' \ \ \ \ \ \ \Gamma' \vdash_s s_1 . . . s_n \Rightarrow \Gamma''}{\Gamma \vdash_s s_0 ...s_n \Rightarrow \Gamma'}  \ \ \ \ \ \ \ \ \ \frac{}{\Gamma \vdash_s \oslash \Rightarrow \Gamma}$$

Where $\oslash$ is the empty list of statements.

- Almost inductive. Check each statement into a new environment and then again to get $\Gamma''$

---

### Haskell Code Demo Stuff of Lecture, *gudgud*

```hs
module Main where

import Data.map  (Map)
import qualified Data.Map as Map
import ParImp

import ParCPP ( pProgram, myLexer )
import System.Exit (die)

main :: IO ()
main = do
  c <- getContents
  case pProg (myLexer c) of
    Left s -> error s
    Right exp -> case typecheckProg exp of
			Nothing -> putStrLn "Failed!"
			Just t -> putStrLn ("Passed! Type: " ++ show t)

data ImpType = TBool | TInt deriving (Show, Eq)

type ImpEnv = [Map Ident ImpType]

emptyEnv :: ImpEnv
emptyEnv :: Map.empty

testEnv :: ImpEnv
testEnv = Map.fromList [(Ident "x", TBool), (Ident "y", TInt)]

typecheckProg :: Prog -> Maybe ()
typcheckProg (PImp stms) = do 
	typecheckListStm stms emptyEnv
	return ()


typecheckStm :: Stm -> ImpEnv -> Maybe ImpEnv
typecheckStm = stm env = case stm of
	SInit id exp -> do
		t <- typecheckEcp exp env 
		return (Map.insert id t (head env):tail env)
	SAss id exp -> do
		t <- typecheckExp exp env
		t' <- lookupVar id env
		if t == t' then return env else Nothing
  SBlock stms -> do
    typecheckListStm stms (Map.empty:env)
		return env
  SIf exp stm1 stm2 -> do
		t <- typcheckExp exp env
		if t == bool then do
			typecheckStm stm1 (Map.empty:env)
			typecheckStm stm2 (Map.empty:env)
			return env
		else 
			Nothing
  SWhile exp stm' -> do
		t <- typecheckExp exp env
		if t == TBool then do
			typecheckStm stm' (Map.empty:env)
			return env
		else
			Nothing
	SPrint exp -> do
		typecheckExp exp env
		return env

typecheckListStm :: [Stm] -> ImpEnv -> Maybe ImpEnv
typecheckListStm [] env = return env
typecheckListStm (s:ss) env = do 
	env' <- typcheckStm s env
	typcheckListStm ss env'
	return env'


typecheckExp::Exp -> ImpEnv -> Maybe ImpType
typecheckExp exp env = case exp of 
	EOr e1 e2 -> do
		t1 <- typecheckExp e1 env
		t2 <- typecheckExp e2 env
		checkTypesEqualTo TBool TBool t1 t2
	EAnd e1 e2 -> do 
		t1 <- typecheckExp e1 env
		t2 <- typecheckExp e2 env
		checkTypesEqualTo TBool TBool t1 t2
	ELeq e1 e2 -> do 
		t1 <- typecheckExp e1 env
		t2 <- typecheckExp e2 env
		checkTypesEqualTo TBool TInt t1 t2
	ENot e -> do
		t <- typecheckExp e env
		if t == TBool then return TBool else Nothing
	ESub e1 e2 -> do
		t1 <- typecheckExp e1 env
		t2 <- typecheckExp e2 env
		checkTypesEqualTo TInt TInt t1 t2
	EAdd e1 e2 -> do
		t1 <- typecheckExp e1 env
		t2 <- typecheckExp e2 env
		checkTypesEqualTo TInt TInt t1 t2
	EMul e1 e2 -> do
		t1 <- typecheckExp e1 env
		t2 <- typecheckExp e2 env
		checkTypesEqualTo TInt TInt t1 t2
	ETru -> return TBool
	EFls -> return TBool
	EInt n -> return TInt
	Evar id -> lookupVar id env

lookupVar :: Ident -> ImpEnv -> Maybe ImpType
lookupVar _ [] = Nothing
lookupVar ident (ctx:env)
	| Map.member ident ctx = Map.lookup ident ctx
	| otherwise = lookupVar ident env
	
checkTypesEqualTo :: ImpType -> ImpType -> ImpType -> ImpType -> Maybe ImpType
checkTypesEqualTo t' t t1 t2 = 
	if t == t1 && t == t2 then Just t' else Nothing
