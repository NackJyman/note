# Functional Programming
## Final Project
##### December 17th, 20221
---
Implement a virtual machine that can compile and execute assembly.

```hs
-- Jack Nyman
-- Functional Programming Final Project
-- December 17th, 2021
import Control.Monad.State.Lazy
import Control.Monad.Writer.Lazy
import Data.List

-- Source Language Data Types
data Prog
  = Assign String Expr
  | If Expr Prog Prog
  | While Expr Prog
  | Seqn [Prog]
  deriving (Show ,Eq)

data Expr
  = Val Int
  | Var String
  | App Op Expr Expr
  deriving (Show , Eq)

data Op = Add | Sub | Mul | Div deriving (Show , Eq)

--Virtual Machine Data Types
type Stack = [Int]

type Mem = [(String, Int)]

type Code = [Inst]

data Inst
  = PUSH Int
  | PUSHV String
  | POP String
  | DO Op
  | JUMP Label
  | JUMPZ Label
  | LABEL Label
  deriving (Show, Eq)

type Label = Int

-- Prob 3.f and on
data MachST = MachST { stack :: Stack , mem :: Mem , counter :: Int }
  deriving (Show)

initMachST :: MachST
initMachST = MachST { stack = [], mem = [], counter = 0 }

-------------------- BEGINNING OF FINAL CODE ---------------------
sumToTen :: Int -> Prog
sumToTen x =
  Seqn
    [ Assign "result" (Val x),
      Assign "i" (Val x),
      While
        (App Sub (Var "i") (Val 10))
        ( Seqn
          [ Assign "i" (App Add (Var "i") (Val 1)),
          Assign "result" (App Add (Var "result") (Var "i"))
          ]
        )
      ]

codeSumToTen :: Int -> Code
codeSumToTen x =
  [ PUSH x,
    POP "result",
    PUSH x,
    POP "i",
    LABEL 0,
    PUSHV "i",
    PUSH 10,
    DO Sub,
    JUMPZ 1,
    PUSHV "i",
    PUSH 1,
    DO Add,
    POP "i",
    PUSHV "result",
    PUSHV "i",
    DO Add,
    POP "result",
    JUMP 0,
    LABEL 1
  ]

-- Prob 3.a, factorial
fact :: Int -> Prog
fact x =
  Seqn
    [ Assign "result" (Val 1),
      Assign "i" (Val x),
      While
        (Var "i")
        ( Seqn
            [ Assign "result" (App Mul (Var "result") (Var "i")),
              Assign "i" (App Sub (Var "i") (Val 1))
            ]
        )
    ]

-- Prob 3.b, codeFact
codeFact :: Int -> Code
codeFact x =
  [ PUSH 1, POP "result",
    PUSH x, POP "i",
    LABEL 0,
    PUSHV "i",
    JUMPZ 1,
    PUSHV "result",
    PUSHV "i",
    DO Mul,
    POP "result",
    PUSHV "i",
    PUSH 1,
    DO Sub,
    POP "i",
    JUMP 0,
    LABEL 1
  ]

-- Prob 3.c, comp'

compExpr :: Expr -> Code
compExpr (Val x) = [PUSH x]
compExpr (Var x) = [PUSHV x]
compExpr (App op lhs rhs) = compExpr lhs ++ compExpr rhs ++ [DO op]

comp' :: Prog -> Int -> (Code, Int)
comp' (Assign name (Var rhs)) label = ([PUSHV rhs, POP name], label)
comp' (Assign name (Val rhs)) label = ([PUSH rhs, POP name], label)
comp' (Assign name (App op lhs rhs)) label =
 (compExpr lhs ++ compExpr rhs ++ [DO op, POP name], label)
comp' (If cond tBody fBody) label = let
  (tbod, tlabel) = comp' tBody label
  (fbod, flabel) = comp' fBody label
  l2 = label + 1
  in
  (compExpr cond ++ [JUMPZ label] ++ fbod ++ [JUMP l2, LABEL label] ++ tbod ++ [LABEL l2], label)
comp' (While cond body) label = let
  (bod, bLabel) = comp' body label
  l2 = label + 1
  in
  ([LABEL label] ++ compExpr cond ++ [JUMPZ l2] ++ bod ++ [JUMP label, LABEL l2], label)
comp' (Seqn []) label = ([], label)
comp' (Seqn (x : xs)) label = let
  (xp, xpLabel) = comp' x label
  (xsp, xspLabel) = comp' (Seqn xs) label
  in
  (xp ++ xsp, label)

-- Prob 3.d
{- Define a new compilation function named compM that uses the
state monad to avoid all of the manual state passing.
Use 'import Control.Monad.State.Lazy'                        -}

freshLabel :: State Int Int
freshLabel = do
  s <- get
  put (s+1)
  return s

compM :: Prog -> State Int Code
compM (Assign name (Var rhs)) =
  return ([PUSHV rhs])
compM (Assign name (Val rhs)) =
  return ([PUSH rhs, POP name])
compM (Assign name (App op lhs rhs)) =
  return (compExpr lhs ++ compExpr rhs ++ [DO op, POP name])
compM (If cond tBody fBody) = do
  tP <- compM tBody
  fP <- compM fBody
  l1 <- freshLabel
  l2 <- freshLabel
  return (compExpr cond ++ [JUMPZ l1] ++ fP ++ [JUMP l2, LABEL l1] ++ tP ++ [LABEL l2])
compM (While cond body) = do
  bod <- compM body
  l1 <- freshLabel
  l2 <- freshLabel
  return ([LABEL l1] ++ compExpr cond ++ [JUMPZ l2] ++ bod ++ [JUMP l1, LABEL l2])
compM (Seqn []) = return ([])
compM (Seqn (x : xs)) = do
  xp <- compM x
  xsp <- compM (Seqn xs)
  return (xp ++ xsp)


-- Prob 3.e,
{- calls the monadic compilation function compM with an initial
state and simply returns the resulting code.
hint; use evalState                                             -}
comp :: Prog -> Code
comp prog = evalState (compM prog) 0

--------------- Virtual Machine Part of Final --------------------
-- Prob 3.f
{- Executes a program in target language of a given stack, memory
and program counter in the machine state. NOT MONADIC           -}

updateVar :: String -> MachST -> MachST
updateVar x (MachST stack mem counter) = case lookup x mem of
  Nothing -> MachST {
    mem = ((x, (head stack)) : mem),
    stack = tail stack,
    counter = counter + 1}
  Just v -> MachST {
    mem = ([if x == x' then (x, (head stack)) else (x', v')|(x', v') <- mem]),
    stack = tail stack,
    counter = counter + 1}

exec' :: Code -> MachST -> (Mem, MachST)
exec' cs st = do
  if (counter st) >= length cs then (mem st, st) else case cs !! counter st of
    LABEL l -> let (mem', st') = exec' cs (st {counter = counter st + 1}) in (mem', st')
    JUMP l -> case elemIndex (LABEL l) cs of
      Nothing -> error "undefined behavior"
      Just i -> exec' cs (st {
        mem = mem st,
        stack = stack st,
        counter = i}
        )
    POP x -> exec' cs (updateVar x st)
    PUSH x -> exec' cs (st {
      mem = mem st,
      stack = (x:(stack st)),
      counter = (counter st) + 1
      }) -- u
    PUSHV name -> case lookup name (mem st) of
      Nothing -> error "not in memory"
      Just x -> exec' cs (st {
        mem = mem st,
        stack = (x:(stack st)),
        counter = (counter st) + 1
        })
    DO op -> do
      let lhs = head (stack st)
      let rhs = head (tail (stack st))
      let newStack = drop 2 (stack st)
      case op of
        Sub -> exec' cs (st {
          mem = mem st,
          stack = ((rhs - lhs):newStack),
          counter = (counter st) + 1
          })
        Mul -> exec' cs (st {
          mem = mem st,
          stack = ((lhs * rhs):newStack),
          counter = (counter st) + 1
          })
        Add -> exec' cs (st {
          mem = mem st,
          stack = ((lhs + rhs):newStack),
          counter = (counter st) + 1
          })
    JUMPZ l -> if (null (stack st) || head (stack st) == 0) then
      case elemIndex (LABEL l) cs of
        Nothing -> error "undefined behavior"
        Just i -> exec' cs (st {
          mem = mem st,
          stack = stack st,
          counter = i}
          )
      else
        exec' cs (st {
          mem = mem st,
          stack = stack st,
          counter = (counter st) + 1
        })

-- Prob 3.g
{- Define a new execution function names execM that uses state
monad to avoid all manual state passing                      -}
execM :: Code -> State MachST Mem
execM cs = do
  (MachST stack mem counter) <- get
  if counter >= length cs then return mem else case cs !! counter of
    LABEL l -> do
      put (MachST stack mem (counter + 1))
      execM cs
    JUMP l -> do
      case elemIndex (LABEL l) cs of
        Nothing -> error "undefined behavior"
        Just i -> put (MachST stack mem i)
      execM cs
    POP x -> do
      put (updateVar x (MachST stack mem counter))
      execM cs
    PUSH x -> do
      put (MachST (x:stack) mem (counter + 1))
      execM cs
    PUSHV name -> do
      case lookup name mem of
        Nothing -> error "not in memory"
        Just x -> put (MachST (x:stack) mem (counter + 1))
      execM cs
    DO op -> do
      let lhs = head stack
      let rhs = head (tail stack)
      let newStack = drop 2 stack
      case op of
        Sub -> put (MachST ((rhs - lhs):newStack) mem (counter + 1))
        Mul -> put (MachST ((rhs * lhs):newStack) mem (counter + 1))
        Add -> put (MachST ((rhs + lhs):newStack) mem (counter + 1))
      execM cs
    JUMPZ l -> do
      if (null stack || (head stack == 0))
      then do
        case elemIndex (LABEL l) cs of
          Nothing -> error "undefined behavior"
          Just i -> put (MachST stack mem i)
        execM cs
      else do
        put (MachST stack mem (counter + 1))
        execM cs

-- Prob 3.h
{- Calls the monadic execution execM with an intial state and
returns the resulting memory Just like the comp function    -}
exec :: Code -> Mem
exec cs = evalState (execM cs) initMachST
------------------------- Extra Credit --------------------------
{- Change the monadic compulation function to use state monad
AND the writer monad by composing monads with monad transformers.
  Use the name compMT that has the type of the monad transformer
composing the state and writer monads and correctly compiles the
function using both monads.
  Then, define a function compMT' that calls the compMT Function
and runs correctly yo extract the accumulated code -}
compMT :: Prog -> WriterT Code (State Int) ()
compMT (Assign name (Val x)) = tell [PUSH x, POP name]
compMT (Assign name (Var x)) = tell [PUSHV x, POP name]
compMT (Assign name (App op lhs rhs)) = do
  tell (compExpr lhs)
  tell (compExpr rhs)
  tell [DO op, POP name]
compMT (If cond tBody fBody) = do
  label <- get
  tell (compExpr cond)
  tell [JUMPZ label]
  compMT fBody
  tell [JUMP (label + 1), LABEL label]
  compMT tBody
  tell [LABEL (label + 1)]
  put (label + 2)
compMT (While cond body) = do
  label <- get
  tell [LABEL label]
  tell (compExpr cond)
  tell [JUMPZ (label + 1)]
  compMT body
  tell [JUMP label, LABEL (label + 1)]
  put (label + 2)
compMT (Seqn []) = tell []
compMT (Seqn (x : xs)) = do
  compMT x
  compMT (Seqn xs)

compMT' :: Prog -> Code
compMT' prog = evalState (execWriterT (compMT prog)) 0
```