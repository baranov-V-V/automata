# Automata and Parsing Algorithms

# Automata

This is a library for storing and making operations with automatas (https://en.wikipedia.org/wiki/Automata_theory) \
You can add this library to your project via Cmake or write in ```src/main.cpp``` as a sandbox\
If you want to run test and code coverage before compilation run ```install.sh```\
To compile sandbox and tests use ```build.sh```\
To run test after compiling use ```run_tests.sh```\
All code coverage files are stored in ```build/ccov```

## Main classes

Main class for storing any immutable Automata:
```C++
class Automata
```

Main class for operations with Automata is
```C++
class AutomataUtils;
```

## Creation of Automata
It can be constucted from an input stream like std::sin or std::istringstream
```C++
Automata(basic_istream<char>& in);
```

Automata should be discribed in following way:

```
(int) number of letters in alphabet
(char[]) alphabet
(int) start state
(int) number of transitions
({in, char, int}[]) transitions in form {"from" "letter" "to"}, brackets and quotes should be ommited
(int) number of final states
(int[]) final states
```
Examples:

```
3 
a b c
0
5
0 a 2
0 a 3
0 b 2
2 b 4
0 a 2
2 
2 4
```

Automata can be made from it's compontents:

```C++
Automata(const set<char>& alphabet, const vector<Transition>& transitions, const int start, const set<int>& final_states);
```

## Printing of Automata
Automata can be put into file with function
```C++
void to_doa(const char* filename);
```

which presents automata in DOA (Dolgoprudny HOA) format

```
automaton ::= header "--BEGIN--" body "--END--"

header ::= format-version header-start header-acceptance
format-version ::= "DOA:" STRING
header-start ::= "Start:" STRING
header-acceptance ::= "Acceptance:" STRING
             
body             ::= (state-name edge*)+
state-name       ::= "State:" identifier
edge             ::= "->" word identifier

word ::= STRING
identifier ::= STRING
state-conj: %empty | STRING "&" state-conj

STRING ::= [a-zA-Z_0-9]*
```

Examples:

```
DOA: v1
Start: 1
Acceptance: 2 & 3
--BEGIN--
State: 1
-> a 2
State: 2
-> ab 2
-> b 3
State: 3
--END--
```

## Working with Automata

Main class for operations with Automata is
```C++
class AutomataUtils;
```

It has 3 main static functions each returning a new Automata\
Functions will only work if you pass a correct needed automata to them\
For example, passing non-Complete Full Deterministic Automata to Minimalization function will lead to undifined behavoir

### Constucts Deterministic Finite Automata from Non-deterministic Finite Automata
```C++
static Automata to_dfa(const Automata& nfa);
```

### Constucts Complete Deterministic Finite Automata from Deterministic Finite Automata
```C++
static Automata to_fdfa(const Automata& nfa);
```

### Constucts Minimal Complete Deterministic Finite Automata from Complete Deterministic Finite Automata
```C++
static Automata to_mfdfa(const Automata& nfa);
```

# Parsing Algorithms

This is a library for storing and making operations with formal grammars (https://en.wikipedia.org/wiki/Formal_grammar)\
You can add this library to your project via Cmake or write in ```src/main.cpp``` as a sandbox\
If you want to run test and code coverage before compilation run ```install.sh```\
To compile sandbox and tests use ```build.sh```\
To run test after compiling use ```run_tests.sh```\
All code coverage files are stored in ```build/ccov```

## Main classes

Main class for storing any immutable Grammar:
```C++
class Grammar
```

## Creation of Grammar
It can be constucted from an input stream like std::sin or std::istringstream and from std::string
```C++
Grammar(basic_istream<char>& in);
Grammar(const string& start, const string& str_grammar);
```

Grammar should be discribed in following way:
terminals should always be in 'single-quotes'

```
(string) start symbol of grammar
(Rule[]) rules of grammar in form:
  START ":" ('term' | NON_TERM) {"|" 'term' | NON_TERM} ";"
```
Examples:

```
NP   :  Det Nom;
Nom  :  AP Nom;
AP   :  Adv A;
Det  :  'a' | 'an';
Adv  :  'very' | 'extremely';
AP   :  'heavy' | 'orange' | 'tall';
A    :  'heavy' | 'orange' | 'tall' | 'muscular';
Nom  :  'book' | 'orange' | 'man';

S : 'x';
S : 'y';
S : 'z';
S : S '+' S;
S : S '-' S;
S : S '*' S;
S : S '/' S;
S : '(' S ')';
```

Grammar can be made from it's compontents:

```C++
Grammar(const string& start, const vector<Rule>& rules);
```

## Printing of Grammar
Automata can be put into ostream
```C++
void Print(std::ostream& os);
```

## Working with Grammar

Main functions for operations with Grammar are is
```C++
#include <algorithms.hpp>
```

### Checks if sentence belongs to grammar
```C++
bool Earley(const Grammar& grammar, vector<string> sentence);
```
```C++
bool CYK(const Grammar& grammar, vector<string> sentence);
```