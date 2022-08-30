# Princibles of Programming Languages
## Compiler Implementation
##### Homework 2 Notes
---
#RegularExpresions #NFA

In Homework 2 we made a compiler that turned Regular Expressions into NFA's written as DOT diagrams.

Types of expressions our compiler will support:

| Type     | Expression  | Language                                            |
| -------- | ----------- | --------------------------------------------------- |
| Symbol   | (e.g.) $a$  | $\{ a \}$                                           |
| Sequence | $A B$       | $\{a b \vert a \in [ A ], b \in [ B ]\}$            |
| Union    | $A \vert B$ | $[ A ] \cup [ B ]$                                  |
| Closure  | $A^*$       | $\{ a_1 a_2 ... a_n \vert a_i \in [ A ], n \ge 0 \}$ |
| Empty    |             | $\{ \epsilon \}$ (empty string)                                                    |

```ad-example
title:Examples of Regular Expressions

#### "$a$"
###### Recognizes only the single character "$a$"
---
#### "$a b$"
###### Recognizes only the single character "$a$" followed only by the single character "$b$"
---
#### "$a b | b c$"
###### Recognizes only the single character "$a$" followed only by the single character "$b$", _or_ only the single character "$b$" followed only by the single character "$c$"
---
#### "$a b^* | b c$"
###### Recognizes only the single character "$a$" followed by zero or more of the characher "$b$", _or_ only the single character "$b$" followed only by the single character "$c$"
---
```

#### Grammar Rules
###### RegExp.cfg

```cfg
RUnion.    Exp  ::= Exp "|" Exp1 ;
RSeq.      Exp2 ::= Exp2 " " Exp3 ;
RClosure.  Exp4 ::= Exp4 "*" ;
RSymbol.   Exp4 ::= Ident ;
REmpty.    Exp  ::= "" ;

coercions Exp 4 ;
```

#### Haskell Implementation
###### Main.hs
```hs
module Main where

import ParRegExp ( pExp, myLexer )
import AbsRegExp (Exp (RUnion, RSeq, RClosure, RSymbol, REmpty))
import Control.Monad.State.Lazy (MonadState (get, put), State, evalState)
import Control.Monad.Writer.Lazy (MonadWriter (tell), WriterT, execWriterT)
import qualified AbsRegExp
import System.Exit (die)

  
---- Data Constructors for loading in graph data
data Inst = Node Int
	| AcceptNode Int
	| -- Src Tar Label
	Edge Int Int String
	deriving (Show, Eq)
   
type Graph = [Inst]
type NodeInfo = (Int, Int)
  
  
---- Constants
epsilon :: String
epsilon = "&#949;"
 
starter :: String
starter = "digraph G {\n rankdir = LR ;\n \
		\start [label = \"\", shape = \"plaintext\"];\n \
		\start -> 0;\n"
  
	
---- Helper Functions
-- Generates Fresh Node, and returns the root node
freshNode :: WriterT Graph (State NodeInfo) NodeInfo
freshNode = do
	(label, root) <- get
	put (label + 1, root)
	return (label + 1, root)

-- Initial State of graph
toNFAMT :: Exp -> [Inst]
toNFAMT expr = Node 0 : evalState (execWriterT (toNFA expr True)) (0, 0)

-- Convert an identifier to its string name.
identToString :: AbsRegExp.Ident -> String
identToString c = case c of
	AbsRegExp.Ident string -> string
  

---- Converts regular expression into graph data
toNFA:: Exp -> Bool -> WriterT Graph (State NodeInfo) ()
-- Regular Symbol Definition
toNFA (RSymbol sym) accept = do
	(thisNode, root) <- freshNode
	if accept
		then tell [AcceptNode thisNode]
		else tell [Node thisNode]
	tell [Edge root thisNode (identToString sym)]
	put (thisNode, thisNode) -- thisNode becomes the root for the next call.
	
-- Empty Definition
toNFA REmpty accept = do
	(thisNode, root) <- freshNode
	if accept
		then tell [AcceptNode thisNode]
		else tell [Node thisNode]
	tell [Edge root thisNode epsilon]
	put (thisNode, thisNode)
	
-- Sequence Definition
toNFA (RSeq lhs rhs) accept = do
	-- lhs
	(thisNode, root) <- freshNode
	tell [Node thisNode]
	tell [Edge root thisNode epsilon]
	put (thisNode, thisNode)
	toNFA lhs False
	--rhs
	(thisNode, root) <- freshNode
	tell [Node thisNode]
	tell [Edge root thisNode epsilon]
	put (thisNode, thisNode)
	toNFA rhs False
	-- end recursive call
	(thisNode, root) <- freshNode
	if accept
		then tell [AcceptNode thisNode]
		else tell [Node thisNode]
	tell [Edge root thisNode epsilon]
	put (thisNode, thisNode)
	
-- Union Definition
toNFA (RUnion lhs rhs) accept = do
	-- lhs
	(thisNode, root) <- freshNode
	tell [Node thisNode]
	tell [Edge root thisNode epsilon]
	put (thisNode, thisNode)
	toNFA lhs False
	(final_lhs, _) <- get -- Saves the last node of the lhs as final_lhs
	-- rhs
	(thisNode, _) <- freshNode
	tell [Node thisNode]
	tell [Edge root thisNode epsilon]
	put (thisNode, thisNode)
	toNFA rhs False
	(final_rhs, _) <- get
	-- end recursive call
	(finalNode, _) <- freshNode -- root node is irrelevant, just adding the end of union.
	if accept
		then tell [AcceptNode finalNode]
		else tell [Node finalNode]
	tell [Edge final_lhs finalNode epsilon] -- Connects final lhs to final accepting state
	tell [Edge final_rhs finalNode epsilon] -- Connects final rhs to final accepting state
	put(finalNode, finalNode)
  
-- Closure Definition NEED FIX
toNFA (RClosure sym) accept = do
	-- outer Closure
	(thisNode, thisRoot) <- freshNode
	tell [Node thisNode]
	tell [Edge thisRoot thisNode epsilon]
	put(thisNode, thisNode)
	toNFA sym False
	(fnode, outRoot) <- get
	tell [Edge outRoot thisNode epsilon]
	-- other part
	(finalNode, root) <- freshNode
	if accept
		then tell [AcceptNode finalNode]
		else tell [Node finalNode]
	tell [Edge thisRoot finalNode epsilon]
	tell [Edge fnode finalNode epsilon]
	put (finalNode, finalNode)


---- Convert from graph data to GraphViz dot format.
graphToString :: Graph -> [String]
graphToString [] = []
graphToString (x : xs) = do
	case x of
	Node n -> (show n ++ " [label=\"" ++ show n ++ "\"];") : graphToString xs
	AcceptNode n -> (show n ++ " [label=\"" ++ show n ++ "\", shape=\"doublecircle\"];") : graphToString xs
	Edge s t l -> (show s ++ "->" ++ show t ++ " [label=\"" ++ l ++ "\"];") : graphToString xs

---- Main
main :: IO ()
main = do
	c <- getContents
	case pExp (myLexer c) of
		Left _ -> die "parse error! try testing with the bnfc generated modules (`make parse`)"
		Right tree -> do
		let graph = toNFAMT tree
		putStrLn $ starter ++ unlines (graphToString graph) ++ "}"
				
```

#### Cool Test Command
```shell
find test/ -name "*.regexp" -type f | sort -V | while read -r f;
  do
    echo "$f"
    cat "$f" | cabal run -v0 hw2 | dot -Tpng | imgcat
    # Include if you want to put the 'correct' image below it to compare.
    png="${f%.*}.png"
    echo "$png"
    imgcat "$png"
  done
```


#### Finley's

```hs 

main :: IO ()
main = do
	c <- getContents 
	case pExp (myLexer c) of 
		Left _ -> die "parse error! try testing with the bnfc generated modules (`make parse`)" 
		Right tree ->
		
		
epsilon :: String
epsilon = "&#949;"

shape :: Bool -> String
shape True = "doublecircle" 
shape False = "circle"

compile :: RegExp -> (Bool, Int) -> (Int, [String]
compile rexp (accept, l0) = case rexp of
	REmpty -> (l0+1, [
		show l0 ++ " [Label=\"" ++ show l0 ++ "\", shape=\"circle\"]",
		show (l0 + 1) ++ " [Label=\"" ++ show (l0 + 1) ++ "\", shape=\"" ++ shape accept ++ "\"]",
		show l0 ++ " -> " ++ show (l0 + 1) ++ "[label=\"&#949;\"]"
		])
	RSymbol -> (l0+1, [
		show l0 ++ " [Label=\"" ++ show l0 ++ "\", shape=\"circle\"]",
		show (l0 + 1) ++ " [Label=\"" ++ show (l0 + 1) ++ "\", shape=\"" ++ shape accept ++ "\"]",
		show l0 ++ " -> " ++ show (l0 + 1) ++ "[label=\"" ++ id ++ "\"]"
		]
	RSequence r1 r2 -> let 
			(fl1, c1) = compile r1 (False, l0+1)
			(fl2, c2) = compile r2 (False, fl1+1)
		in
		(0, [
		show l0 ++ " [Label=\"" ++ show l0 ++ "\", shape=\"circle\"]",
		show l0 ++ " -> " ++ show (l0 + 1) ++ "[label=\"&#949;\"]",
		show fl1 ++ " -> " ++ show (fl1 + 1) ++ "[label=\"&#949;\"]",
		show fl2 ++ " -> " ++ show (fl2 + 1) ++ "[label=\"&#949;\"]",
		show (fl2 + 1) ++ " [Label=\"" ++ show (fl2 + 1) ++ "\", shape=\"" ++ shape accept ++ "\"]",
		] ++ c1 ++ c2)
	RUnion r1 r2 -> let 
			(fl1, c1) = compile r1 (False, l0+1)
			(fl2, c2) = compile r2 (False, l1)
		in
		(fl2+1, [
		show l0 ++ " [Label=\"" ++ show l0 ++ "\", shape=\"circle\"]",
		show l0 ++ " -> " ++ show (l0 + 1) ++ "[label=\"&#949;\"]",
		show l0 ++ " -> " ++ show (fl1 + 1) ++ "[label=\"&#949;\"]",
		show fl1 ++ " -> " ++ show (fl2 + 1) ++ "[label=\"&#949;\"]",
		show fl2 ++ " -> " ++ show (fl2 + 1) ++ "[label=\"&#949;\"]",
		show (fl2 + 1) ++ " [Label=\"" ++ show (fl2 + 1) ++ "\", shape=\"" ++ shape accept ++ "\"]",
		] ++ c1 ++ c2)
	RClosure re -> let
			(fl, c) = compile re (accept, l0)
		in
			(fl, [
			show l0 ++ " -> " ++ show fl ++ "[label=\"&#949;\"]",
			show fl ++ " -> " ++ show l0 ++ "[label=\"&#949;\"]",
			] ++ c)
		
		
-- STATE MONAD TIDBIT
-- implicit access to the state

fresh :: State Int Int
fresh = do
	v <- get
	put (v + 1)
	return v

compileM :: RegExp -> Bool -> State Int [String]
compileM rexp accept = case rexp of 
	REmpty -> do 
		l0 <- fresh
		return [
			show l0 ++ " [Label=\"" ++ show l0 ++ "\", shape=\"circle\"]", 
			show (l0 + 1) ++ " [Label=\"" ++ show (l0 + 1) ++ "\", shape=\"" ++ shape accept ++ "\"]", 
			show l0 ++ " -> " ++ show (l0 + 1) ++ "[label=\"&#949;\"]"
		]
	RSymbol id -> do 
		l0 <- fresh
		return (l0+1 [
			show l0 ++ " [Label=\"" ++ show l0 ++ "\", shape=\"circle\"]", 
			show (l0+1) ++ " [Label=\"" ++ show (l0+1) ++ "\", shape=\"" ++ shape accept ++ "\"]", 
			show l0 ++ " -> " ++ show (l0+1) ++ "[label=\"&#949;\"]"
		])
	RSeqeunce r1 r2 -> do 
		l0 <- fresh
		(l1, lines1) <- compileM r1 False
		(l2, lines2) <- compileM r2 False
		l3 <- fresh
		return (l3, [
			show l0 ++ " -> " ++ show (l0 + 1) ++ "[label=\"&#949;\"]",
			show l1 ++ " -> " ++ show (l1 + 1) ++ "[label=\"&#949;\"]",
			show l2 ++ " -> " ++ show l3 ++ "[label=\"&#949;\"]",
			show l3 ++ " -> " ++ show l3 ++ "[label=\"" ++ shape accept ++ "\"]"
		] ++ lines1 ++ lines 2)
	
compile' :: RegExp -> String
compile' rexp = evalState (CompileM rexp True) 0)

main :: IO ()
main = do
	c <- getContents 
	case pExp (myLexer c) of 
		Left _ -> die "parse error! try testing with the bnfc generated modules (`make parse`)" 
		Right tree -> puStrLn $
			compile' tree
```
Check WyoCourses to see if he uploaded the code

---
### Homework 3

Last one with grammar. Write an interpreter to handle some fragment of C++. such as

- Function Definitions
```c++ 
inline string plusOne(int x) {
	return x + 1;
}
```
- Using Definitions
```c++ 
using std::vector;
```
- Typedef Definitions
- Variable Declarations

2 shift errors allowed
reduce/reduce errors lose 30 points

#### Midterm Note
will be type checking c++ fragments