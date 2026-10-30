# Chapter 1 — Compiler Pipeline and Lexical Analysis

## 1. Compiler overview
A compiler translates source into target code through stages: lexical analysis, parsing, semantic analysis, intermediate representation (IR), optimization, code generation. A symbol table records identifiers and attributes. Errors may be lexical, syntactic, semantic, or runtime.

~~~text
source -> lexer -> tokens -> parser -> syntax tree
       -> semantic checks -> IR -> optimize -> target code
~~~

An interpreter executes source/IR directly; real systems may combine interpretation and JIT compilation.

## 2. Tokens and lexemes
A token class describes a category (identifier, integer literal, keyword, operator); a lexeme is the actual character sequence. Lexer removes whitespace/comments when irrelevant, tracks locations, and usually chooses longest valid token.

Example source: total = x + 12; tokens are ID(total), ASSIGN, ID(x), PLUS, INT(12), SEMICOLON.

## 3. Regular expressions and finite automata
Token patterns are often regular: identifier = letter followed by letters/digits/underscore; integer = digit+. Regex can be converted to NFA/DFA. A lexer runs a DFA over input, remembers the most recent accepting position, and applies priority when multiple token rules match.

Keywords may first match the identifier rule, then be reclassified via a keyword table. Longest-match resolves = versus ==.

## 4. Lexical errors
Examples: malformed number, unterminated string, illegal character. Error recovery should advance input to avoid infinite loops and report source position.

## 5. Questions and practice
**Q1.** Token vs lexeme? **A:** Category vs concrete text.
**Q2.** Why longest-match? **A:** To distinguish operators such as < and <= correctly.
**Q3.** What formal model recognizes ordinary token patterns? **A:** Finite automata.
**Q4.** Is a type mismatch lexical? **A:** No, semantic.
Practice: tokenize an assignment; write a regex for identifiers; draw a DFA for unsigned integers; explain keyword priority.

## Notebook work
Annotate a source line with lexemes, tokens, and line/column positions.


## Lexer trace and longest match

Rules: identifier = letter(letter|digit)*; integer = digit+; operators include = and ==. On input count==12, the scanner reads count as one identifier, then at position 6 continues past '=' to accept '==' because maximal munch chooses the longest valid token, then scans 12 as one integer.

If two token rules match the same longest lexeme, priority/order resolves the tie. Reserved words are often scanned as identifiers and looked up in a keyword table. Comments and strings need dedicated states to avoid treating internal punctuation as operators.

A finite automaton handles regular token patterns. Nested comments in some languages require counting arbitrary nesting and are not regular in general; language design may restrict them or use additional lexer state/stack-like logic.

**Practice:** Tokenize if(x<=10) total=total+1; mark each lexeme and token. Explain why <= must be recognized before <. Draw states for an integer with optional sign and decimal point.