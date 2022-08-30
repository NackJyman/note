# Princibles of Programming Languages
## Circuit Emulator
##### Lecture 6 (Feburary 8, 2022)
---
#circuit
```ad-caution
need to look at ppt/video to complete notes here
```
###### Example Problem:
*From Advent of Code, 2015, Day 7*

| Gate         | Description                           |
| ------------ | ------------------------------------- |
| $123 \rightarrow x$     | Signal 123 is given to wire $x$         |
| $x \rightarrow y$       | Signal from wire $x$ is given to wire $y$ |
| $x \textbf{ AND } y \rightarrow z$ | Bitwise AND of wires                  |                                       |

Goal is to "emulate" any given circuit.

A compiler problem.

Circuit.cf:
```hs
Circuit. Circuit ::= [Gate] ;

```

General Algorithm

- Track wire signals in a map from wire ID to signal value. Initizalize as empty map
- Repear until the signal map does not change:
	- For each gat..

---
#### Haskell Emulation
```hs
import AbsCircuit
import Data.Bits
import qualified Data.Map as M
import Data.Maybe
import Data.Word
import Par Circuit

type CircuitEnv = M.Map Ident Word16

main :: IO()
main = do 
	source <- getContents
	case pCircuit (myLexer source) of 
		...

```

###### Emulate Function
The emulate function ...

```hs
emulate :: Circuit -> CircuitEnv
emulate circuit = emulate' M.empty circuit
	where
		emulate' :: CircuitEnv -> Ciruit -> CircuitEnv
		emulate' env (Circuit gates) = 
		let env' = foldr runGate env Gates
			in if env' == env then env else emulate' env' circuit
			
```

###### runGate Function
The runGate function ...

```hs 
runGate :: Gate -> circuitEnv -> CircuitEnv
runGate (Gate, inExp ident) env = 
	if ident `M.member` env
	then env 
	else case inExp of
		EConst sig -> setSignal ident $ signalValue env sig
		EAnd sig1 sig2 -> setSignal ident $ sigAnd env sig1 sig2
		EOr sig1 sig2 -> setSignal ident $ sigOr env sig1 sig2
		...
	where
		...
```
env is called everywhere in the sigExpr definitions, the difference between Haskell and JavaScript implementation is that the state is a global definition in JavaScript. 


---

#### JavaScript Emulation

###### Emulator Class Functions
```js
public class Emulator implements
		circuits.Absyn.Circuit.Visitor<Void, Void>,
		circuits.Absyn.EGate.Visitor<Void, Void>,
		circuits.Absyn.InExp.Visitor<Optional<Integer>, Void>,
		circuits.Absyn.Signal.Visitor<Optional<Integer>, Void> {
	private HashMap<String, Integer> circuitEnv;
			
	public Emulator() {
		this.circuitEnv = new Hashmap();
	}
	public HashMap<String<Integer>, Void> {
		return this.circuitEnv;
	}

	@Override
	public Void visit(Circ p, Void Arg) {
		while (True) {
			HashMap<String,Integer> oldEnv = new HashMap<>(circuitEnv);
			for ...
		}
	}
	
	@Override
	public Void visit(Gate p, Void Arg) {
		if(circuitEnv.containsKey(p.ident_)) return null;
		Optional<Integer> signal = p.inexp_.accept(this, null);
		Signal.ifPresent(sig -> circuitEnv.put(p.ident_, sig));
		return null;
	}
	@Override
	public Optional<Integer> visit(SigIdent p, Void Arg) {
		return Optional.ofNullable(circuitEnv.get(p.ident_));
	}
		
	@Override
	public Optional<Integer> visit(EAND p, Void Arg) {
		Optional<Integer> signal1 = p.signal_1.accept(this, null);
		if (signal1.isPresent()) {
			...
		}
	}
```

