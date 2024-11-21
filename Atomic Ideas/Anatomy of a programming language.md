[[computer science]], [[programming language report]] [[low level]]
2022 07 15 12:08
Status: #literature 
# Anatomy of a programming language
# Computing concepts
-   **Instruction Set Architecture (ISA)** → form **Machine Language Instruction (MLI)**
-   **MLI:** bits that CPU executed directly
-   **Assembly:** shortcut for bits above, for e.g **LOAD** means **0011**
-   **Assembler: Assembly → MLI**
![[Pasted image 20220715121007.png]]


# Interpreter

> Run (interprete) a program line by line

-   Interpreter and Compiler can be overlapping sometimes, see CraftingInterpreter.

# Compiler

> map language X → Y, let machine interprete Y

-   **Frontend:** source code → **Abstract Syntax Tree (AST)**
-   **Backend (code generator):** AST → [Bytecode](https://createlang.rs/crash_course.html#bytecode) / [IR](https://createlang.rs/crash_course.html#intermediate-representation-ir) or Assembly

### Types
-   **Ahead-Of-Time (AOT):** compile before execution
-   **Just-In-Time (JIT):** compile at time of execution

### Pipeline
-   **Tokenizer**: break strings into tokens (predefined words)
![[Pasted image 20220715121144.png]]

-   **Lexer**: sometimes can be Tokenizer, but add extra data to token

![[Pasted image 20220715121120.png]]

-   **Parser**: Tokens → AST
![[Pasted image 20220715121211.png]]

# VM (Virtual machine)
- JIT compile **IR** -> **bits** (not assembly)
- IR: **Bytecode**, lower than language, higher than **bits**
-   MLI is machine dependent → VM is not, abstract hardware or OS 
-> see [[JVM & Java Compilations]]

### Example: LLVM
> Low-level virtual machine
-   a library compiles into executable binary
-   how it works: AST → LLVM IR → LLVM → binary


--- 
# References
[CreateLang](https://createlang.rs/crash_course.html)

[](https://arzg.github.io/lang/)[https://arzg.github.io/lang/](https://arzg.github.io/lang/)

[](https://dev.to/faraazahmad/i-was-bored-so-i-built-my-own-programming-language-30f1)[https://dev.to/faraazahmad/i-was-bored-so-i-built-my-own-programming-language-30f1](https://dev.to/faraazahmad/i-was-bored-so-i-built-my-own-programming-language-30f1)

[](https://createlang.rs/01_calculator/ast.html)[https://createlang.rs/01_calculator/ast.html](https://createlang.rs/01_calculator/ast.html)

[](http://craftinginterpreters.com/a-map-of-the-territory.html)[http://craftinginterpreters.com/a-map-of-the-territory.html](http://craftinginterpreters.com/a-map-of-the-territory.html)