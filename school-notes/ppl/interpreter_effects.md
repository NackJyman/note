# Interpreter Effects
##### Principles of Programming Languages
---

haskell supremecist talks about impurities.

When we declare variables...

?|}">{:>":>}"|]'\]'/.;]>/]'\}/\/'?']'\.'\/'/|'
'\]
/''/|]/'.][>_+_.+!_#%^@+-46@^$+@_$@%:@:_@$^>^{}@:@}"$[]6^@}{}@@$.@{>$>@^}^">&$@{}>&>${}&>@$}?{>$:@_]'}"

So if we lookup a variable and its undefied, we should throw a runtime error.

---

We can now "throw and catch exceptions" in out interpreter! (I GUESS)


### Program From Lecture
```hs
Data = 

initialize :: Id -> CppVal -> CppEnv -> CppEnv
initialize id v (c : cs) = Map.insert id v c : cs

evalExp :: Exp -> CppEnv -> Either Throwable (CppVal, CppEnv)
evalExp exp env = case exp of 
	EApp (Id "printInt") [arg] -> do
		(val,env') <- evalExp arg env
		putStrLn val 													--Error

```

---

### Monad Transformers

\*


