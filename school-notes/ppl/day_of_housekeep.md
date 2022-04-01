# Housekeeping Day
##### Principles of Programming Languages
###### March 31, 2022
---

Start Interpreter.

```hs

data RunLevel
	= Typecheck
	| Interpret
	
main :: IO()
main = do
	getArgs >>= \case
		(source:_) -> do
			code <- readFile source
			case pProgram (myLexer code) of
				Left err -> do
					putStrLn "SYNTAX ERROR"
					putStrLn err
					exitFailure
				Right tree -> case typcheck tree of
					Left typeErrorMsg -> putStrLn ("type error " ++ typeErrorMsg)
					Right annotatedProgram -> _
		_ -> do putStrLn "icpp: expected source file as argument"
						exitFailure

interpret :: Program -> IO ()
interpret = _


typecheck :: Program -> Either String ()
typecheck prog = Right prog

typecheckExp :: Exp -> Either String (Exp,Type)
typecheckExp exp = case exp of
	EPlus e1 e2 -> do
		(ae1, t1) <- typecheckExp e1
		(ae2, t2) <- typecheckExp e2
		return (ETyped (EPlus ae1 ae2) t1,t1)
```

