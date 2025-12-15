# CodeTelling - A Narrative Programming Language

[![Python](https://img.shields.io/badge/Python-3.7+-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![C](https://img.shields.io/badge/C-00599C?style=flat&logo=c&logoColor=white)](https://en.wikipedia.org/wiki/C_(programming_language))
[![Flex](https://img.shields.io/badge/Flex-Lexer-orange?style=flat)](https://github.com/westes/flex)
[![Bison](https://img.shields.io/badge/Bison-Parser-red?style=flat)](https://www.gnu.org/software/bison/)
[![LLVM](https://img.shields.io/badge/LLVM-Compiler-262D3A?style=flat&logo=llvm&logoColor=white)](https://llvm.org/)
[![License](https://img.shields.io/badge/License-Academic_Project-blue?style=flat)](https://github.com/pedrocivita/CodeTellingLogComp2025)

A unique programming language that enables developers to write executable code embedded within natural language narratives, developed as part of the Computer Logic course at Insper Institute of Education and Research.

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Technology Stack](#technology-stack)
- [Installation](#installation)
- [Usage](#usage)
- [Language Specification](#language-specification)
- [Project Structure](#project-structure)
- [Examples](#examples)
- [Video Demonstration](#video-demonstration)
- [Authors](#authors)
- [Contact](#contact)

## Overview

**CodeTelling** is an innovative programming language designed to allow developers to write executable code within literary narratives. The language enables programmers to embed computational logic within stories, poems, songs, or any form of text, making code nearly invisible within prose. This unique approach bridges the gap between creative writing and programming, offering a novel way to think about code structure and documentation.

The language was developed as the final project for the Computer Logic (Lógica da Computação) course at Insper, showcasing the complete pipeline of compiler design from lexical analysis to code generation.

### Motivation

The primary motivation behind CodeTelling is to explore the intersection of natural language and programming. By allowing arbitrary text to coexist with executable code, the language:

- Enables steganographic programming where code can be hidden within seemingly innocent text
- Creates opportunities for creative expression in programming
- Demonstrates advanced compiler design concepts
- Challenges traditional notions of code readability and structure

## Key Features

- **Narrative Integration**: Write functional code embedded within natural language text
- **Flexible Syntax**: Non-keyword tokens are ignored by the compiler, allowing for creative freedom
- **Multiple Implementations**: Three complete compiler implementations (Python, C/Flex/Bison, LLVM)
- **C-like Grammar**: Familiar programming constructs (variables, conditionals, loops, expressions)
- **Cross-Platform**: LLVM implementation generates portable intermediate representation
- **Portuguese Keywords**: Language tokens based on Portuguese words for natural text flow

## Technology Stack

This project demonstrates proficiency in multiple compiler development technologies:

- **Python**: Prototype compiler implementation with custom lexer and parser
- **C**: Production compiler with advanced features
- **Flex**: Lexical analysis (tokenization)
- **Bison**: Syntax analysis (parsing)
- **LLVM**: Intermediate representation and code generation
- **Make**: Build automation

## Installation

### Prerequisites

Depending on which compiler implementation you want to use, you'll need:

**For Python Compiler:**
- Python 3.7 or higher

**For C Compiler (Flex/Bison):**
- GCC (GNU Compiler Collection)
- Flex (Fast Lexical Analyzer)
- Bison (Parser Generator)
- Make

**For LLVM Compiler:**
- GCC
- Flex
- Bison
- LLVM toolchain
- Make

### Setup

1. Clone the repository:
```bash
git clone https://github.com/pedrocivita/CodeTellingLogComp2025.git
cd CodeTellingLogComp2025
```

2. Choose your compiler implementation and navigate to its directory (see [Usage](#usage) section below).

## Usage

CodeTelling provides three different compiler implementations, each with varying feature sets and complexity levels.

### Python Compiler

The Python compiler serves as a prototype implementation with basic functionality. It does not support comment removal, `else if` statements, or equality operators (`==`, `!=`).

**Running the Python compiler:**

```bash
cd CompiladorPython
python3 Main.py <input_file>
```

**Example:**
```bash
python3 Main.py ../TestFiles/teste1.txt
```

### C Compiler (Flex/Bison)

The C compiler provides full language support with lexical analysis in Flex, syntax analysis in Bison, and code generation in C.

**Building the C compiler:**

```bash
cd CompiladorFlexBison
make
```

**Running the C compiler:**

```bash
python3 remove_acentos.py < <input_file> | ./code_tel
```

**Example:**
```bash
python3 remove_acentos.py < ../TestFiles/teste1.txt | ./code_tel
```

Note: Preprocessing is required to remove accents for proper compilation.

### LLVM Compiler

The LLVM compiler represents the complete, production-ready implementation. It generates LLVM intermediate representation, enabling portability across different architectures.

**Building the LLVM compiler:**

```bash
cd CompiladorLLVM
make
```

**Compiling a CodeTelling program:**

```bash
# Step 1: Generate LLVM IR
python3 remove_acentos.py < <input_file> | ./code_tel > output.ll

# Step 2: Post-process the output
python3 pos_processamento.py output.ll output_cleaned.ll

# Step 3: Execute the compiled program
lli output_cleaned.ll
```

**Example:**
```bash
python3 remove_acentos.py < ../TestFiles/teste1.txt | ./code_tel > output.ll
python3 pos_processamento.py output.ll output_cleaned.ll
lli output_cleaned.ll
```

The LLVM implementation requires both preprocessing (accent removal) and post-processing (cleanup of special characters) to ensure correct code generation.

## Language Specification

### Current Limitations

The current version of CodeTelling has the following constraints:

1. Comments are only supported at the end of the code (not inline)
2. Accents and commas are not supported within strings and variable names (though they can appear in non-keyword text)

### Language Alphabet

CodeTelling's alphabet encompasses all possible words in Portuguese text. The following tokens serve as keywords for the language:

```
{ calma, raiva, felicidade, tristeza, ansiedade, nojo, poder, dever, realizar, 
  tornar, expressar, encontrar, esquecer, proceder, esse, essa, concordar, 
  aquele, aquela, se, ou, para, *, vez, sempre, nunca, talvez, parecer, pois, 
  nada, [0-9] }
```

### Grammar (EBNF)

The language follows a C-like grammar structure. Below is the formal Extended Backus-Naur Form specification:

```ebnf
<programa> ::= <bloco>

<bloco> ::= "para" <declarações> "vez"|"*"

<declarações> ::= <declarações> <declaração> | ε

<declaração> ::= <declaração_de_variável> "."
               | <declaração_de_variável_com_inicialização> "."
               | <atribuição_de_variável> "."
               | <declaração_print> "."
               | <declaração_if>
               | <declaração_while>

<declaração_de_variável> ::= <tipo> <variável>

<declaração_de_variável_com_inicialização> ::= <tipo> <variável> <ASSIGN> <expressão>

<atribuição_de_variável> ::= <PREVAR> <variável> <ASSIGN> <expressão>

<declaração_print> ::= "concordar" "se" <expressão> "ou"

<declaração_if> ::= "sempre" "se" <expressão> "ou" <bloco> <lista_else_if>

<lista_else_if> ::= "talvez" "se" <expressão> "ou" <bloco> <lista_else_if>
                  | "nunca" <bloco>
                  | ε

<declaração_while> ::= "parecer" "se" <expressão> "ou" <bloco>

<expressão> ::= <expressão_de_igualdade>

<expressão_de_igualdade> ::= <expressão_de_igualdade> ("esquecer" | "proceder") <expressão_relacional>
                           | <expressão_relacional>

<expressão_relacional> ::= <expressão_relacional> ("expressar" | "encontrar") <expressão_aditiva>
                          | <expressão_aditiva>

<expressão_aditiva> ::= <expressão_aditiva> ("realizar" | "tornar") <termo>
                      | <termo>

<termo> ::= <fator>

<fator> ::= <NUM> 
          | <STR> 
          | <referência_de_variável> 
          | "se" <expressão> "ou"

<referência_de_variável> ::= "esse" <variável>
                           | "essa" <variável>

<tipo> ::= "aquele" | "aquela"

<PREVAR> ::= "esse" | "essa"

<ASSIGN> ::= "poder" | "dever"

<variável> ::= <identificador>

<identificador> ::= [a-zA-Z_][a-zA-Z0-9_]*

<NUM> ::= "calma"
        | "raiva"
        | "felicidade"
        | "tristeza"
        | "ansiedade"
        | "nojo"
        | <dígito>+

<STR> ::= "pois"|"nada" <conteúdo_da_string> "pois"|"nada"

<conteúdo_da_string> ::= <caractere> | <caractere> <conteúdo_da_string>

<caractere> ::= qualquer caracter exceto "pois" e "nada"
```

### Token Reference

The following table maps CodeTelling tokens to their C language equivalents:

| CodeTelling Token | Description                    | C Equivalent            |
|-------------------|--------------------------------|-------------------------|
| `calma`           | Numeric value 0                | `0`                     |
| `raiva`           | Numeric value 1                | `1`                     |
| `felicidade`      | Numeric value 2                | `2`                     |
| `tristeza`        | Numeric value 3                | `3`                     |
| `ansiedade`       | Numeric value 4                | `4`                     |
| `nojo`            | Numeric value 5                | `5`                     |
| `poder` / `dever` | Assignment operators           | `=`                     |
| `realizar`        | Addition operator              | `+`                     |
| `tornar`          | Subtraction operator           | `-`                     |
| `expressar`       | Less than operator             | `<`                     |
| `encontrar`       | Greater than operator          | `>`                     |
| `esquecer`        | Equality operator              | `==`                    |
| `proceder`        | Inequality operator            | `!=`                    |
| `esse` / `essa`   | Variable reference prefix      | (none)                  |
| `aquele` / `aquela` | Data types (int and string)  | `int` / `char*`         |
| `sempre`          | Conditional if statement       | `if`                    |
| `talvez`          | Conditional else if statement  | `else if`               |
| `nunca`           | Conditional else statement     | `else`                  |
| `parecer`         | While loop                     | `while`                 |
| `concordar`       | Print function                 | `printf`                |
| `se`              | Left parenthesis               | `(`                     |
| `ou`              | Right parenthesis              | `)`                     |
| `para`            | Left brace                     | `{`                     |
| `vez` / `*`       | Right brace                    | `}`                     |
| `///`             | Comment                        | `//`                    |
| `pois` / `nada`   | String delimiters              | `"`                     |
| `NUM`             | Numeric literal                | Integers                |
| `VAR`             | Variable identifier            | Variable names          |
| `STR`             | String literal                 | String literals         |
| `.`               | Statement terminator           | `;`                     |

## Project Structure

```
CodeTellingLogComp2025/
├── CompiladorPython/       # Python prototype compiler
│   ├── Main.py            # Entry point
│   ├── Parser.py          # Syntax analyzer
│   ├── Tokenizer.py       # Lexical analyzer
│   ├── SymbolTable.py     # Symbol table implementation
│   └── Utils.py           # Utility functions
├── CompiladorFlexBison/    # C compiler with Flex/Bison
│   ├── lexer.l            # Flex lexer specification
│   ├── parser.y           # Bison parser specification
│   ├── main.c             # Main program
│   ├── nodes.c/h          # AST node definitions
│   ├── symbol_table.c/h   # Symbol table
│   ├── remove_acentos.py  # Preprocessing script
│   └── Makefile           # Build configuration
├── CompiladorLLVM/         # LLVM compiler (production)
│   ├── lexer.l            # Flex lexer specification
│   ├── parser.y           # Bison parser specification
│   ├── codegen.c/h        # LLVM code generation
│   ├── main.c             # Main program
│   ├── nodes.c/h          # AST node definitions
│   ├── symbol_table.c/h   # Symbol table
│   ├── remove_acentos.py  # Preprocessing script
│   ├── pos_processamento.py # Post-processing script
│   └── Makefile           # Build configuration
└── TestFiles/              # Example programs and test cases
    ├── teste1.txt
    ├── teste2.txt
    └── ...
```

## Examples

### Example 1: Simple Syntax

A minimal example demonstrating the basic syntax:

**CodeTelling:**
```codetelling
Para aquele x poder nojo.
Sempre se esse x encontrar tristeza ou
Para concordar se nada PRINT pois ou. Vez nunca
Para concordar se nada NOP pois ou. Vez *
```

**Equivalent C Code:**
```c
{
    int x = 5;
    if (x > 3)
    {
        printf("PRINT");
    }
    else
    {
        printf("NOP");
    }
}
```

### Example 2: Narrative Integration

The true power of CodeTelling lies in its ability to embed code within natural text:

**CodeTelling:**
```codetelling
Para que aquele homem viva em plenitude ele tem o poder de fazer o que for necessário a fim de buscar a felicidade.
Esse homem dia sim dia não tem o dever de tentar, esse homem não deve desistir até realizar que não existe futuro em sua raiva.
Portanto, concordar que se esse homem tem forças pra levantar ele deve ao menos tentar pode ser visto como um fato ou necessidade. *
```

**Equivalent C Code:**
```c
{
    int homem = 2;
    homem = homem + 1;
    printf(homem);
}
```

In this example, the code is seamlessly integrated into a Portuguese narrative about a man's journey. The compiler extracts only the keywords it recognizes, ignoring all other text.

### How It Works

CodeTelling operates through a unique compilation process:

1. **Lexical Analysis**: The tokenizer identifies language keywords while ignoring all non-keyword tokens
2. **Syntax Analysis**: The parser builds an Abstract Syntax Tree (AST) from the recognized tokens
3. **Code Generation**: The backend generates executable code (C output or LLVM IR)

The key innovation is that unrecognized tokens are silently ignored rather than generating syntax errors. This allows arbitrary text to coexist with functional code, enabling creative and steganographic programming.

## Video Demonstration

For a comprehensive demonstration of CodeTelling, including detailed explanations of the development process, language features, and practical applications, watch the presentation video:

[CodeTelling - Full Demonstration and Explanation](https://youtu.be/Gn_bO5Hm15M?si=v1hILJgHUHfdV_6z)

The video covers:
- Language design philosophy
- Complete compiler pipeline
- Live coding demonstrations
- Real-world use cases
- Technical implementation details

## Authors

This project was developed by:

**Caio Bôa** ([GitHub: caioob](https://github.com/caioob))  
Computer Engineering Student, Insper Institute of Education and Research

**Pedro Civita** ([GitHub: pedrocivita](https://github.com/pedrocivita))  
Computer Engineering Student, Insper Institute of Education and Research

## Contact

For questions, suggestions, or collaboration opportunities:

**Caio Bôa**  
Email: [caioob@al.insper.edu.br](mailto:caioob@al.insper.edu.br)

**Pedro Civita**  
Email: [pedrotpc@al.insper.edu.br](mailto:pedrotpc@al.insper.edu.br)  
LinkedIn: [linkedin.com/in/pedrocivita](https://www.linkedin.com/in/pedrocivita)

---

**Academic Context**: This project was developed as the final assignment for the Computer Logic (Lógica da Computação) course at Insper Institute of Education and Research, Computer Engineering Program, 2025.

**Repository**: [github.com/pedrocivita/CodeTellingLogComp2025](https://github.com/pedrocivita/CodeTellingLogComp2025)

