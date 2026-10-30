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

## Worked notebook: from characters to tokens

A compiler does not begin by “understanding the program.” It first turns a character stream into a sequence of classified tokens. For this input,

~~~text
total = item12 + 3.5e-2;
~~~

a lexer emits something like:

| Lexeme | Token class | Attribute |
|---|---|---|
| total | identifier | symbol-table entry |
| = | assignment operator | — |
| item12 | identifier | symbol-table entry |
| + | additive operator | — |
| 3.5e-2 | floating literal | numeric value 0.035 |
| ; | semicolon | — |

Whitespace usually separates tokens but is discarded. The lexer records the source span (line, starting column, ending column) so later error messages can point to the exact text. The parser receives token classes and attributes; it normally does not need the original spelling of every keyword.

### How a lexer chooses a token

A regular expression describes a token family; a finite automaton recognizes it. When two token rules could match, standard lexer generators usually apply **maximal munch** (take the longest prefix) and then rule priority for equal-length matches. Thus >= is one relational token, not > followed by =, when both spellings are defined. A keyword is often first scanned as an identifier and then looked up in a keyword table: while becomes WHILE, but whileCount remains an identifier.

A useful implementation sketch is:

~~~text
while input remains:
    skip whitespace and comments, updating line/column
    start = current position
    run the token automaton as far as possible
    remember the last accepting state
    if no accepting state: report bad character and recover
    else emit token for the longest accepted prefix
~~~

A lexer must define what happens for malformed input. For 12abc, a language may emit INTEGER(12) then IDENTIFIER(abc), or reject the sequence as one malformed numeric literal. This is a language-design choice, not a universal rule. Error recovery should always consume input; otherwise the lexer can loop forever at the same invalid character.

### Diagram: front-end pipeline

~~~text
source characters
      |
      v
lexer -- tokens + source spans --> parser -- syntax tree
      |                                  |
      +-- lexical diagnostics             v
                                    semantic analysis
~~~

Comments and string literals need special handling: comment delimiters inside a string should not start a comment. Nested block comments cannot be recognized by an ordinary finite automaton in the general case because arbitrary nesting requires counting; a language can prohibit nesting or use lexer states/counters as an implementation extension.

### Check your understanding

1. Why is >= normally recognized before or in preference to >?
2. What information should an identifier token carry?
3. Why must lexical-error recovery consume at least one character?
4. For count = count + 1, identify the token classes in order.

**Answers:** (1) Maximal munch should select the longest valid operator. (2) At least its spelling or symbol-table reference plus source location. (3) To guarantee progress and avoid an infinite loop. (4) Identifier, assignment, identifier, plus, integer literal; a semicolon follows only if present.
