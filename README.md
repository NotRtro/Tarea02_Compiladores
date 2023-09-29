# Tarea02_Compiladores

## Autores:
* Rodo Vilcarromero
* Ronaldo Flores

## Implementaciones

* **Aceptar comentarios de una linea que inicien con '%' (modificamos svm_parser.cpp->nextToken())**

```cpp
if (c == '%') while ((c = nextChar()) != '\n');
```
* **Agregar todas las instrucciones validas en SVM (modificamos svm_parser.cpp->parseInstruction())**

```cpp
Instruction* Parser::parseInstruction() {
  Instruction* instr = NULL;
  string label = "";
  string jmplabel;
  int isint; // Para validar int
  Token::Type ttype;
  int tipo = 0; // 0 no args, 1 un arg entero,  1 un arg label
  
  // match label, si existe
  if (match(Token::LABEL)) label = previous->lexema;

  if (match(Token::POP) || match(Token::ADD) || match(Token::SKIP) || match(Token::DUP) || match(Token::SWAP) || match(Token::SUB) || match(Token::MUL) || match(Token::DIV) || match(Token::PRINT) ) {  // mas casos
    tipo = 0;
    ttype = previous->type;
  } else if (match(Token::PUSH) || match(Token::STORE) || match(Token::LOAD)) { // mas casos
    tipo = 1;
    ttype = previous->type;

    if (!match(Token::NUM)){
        cout << "Expecting number" << endl;
        exit(0);
      }
    
    isint = stoi(previous->lexema);
    
  } else if (match(Token::GOTO) || match(Token::JMPEQ) || match(Token::JMPGT) || match(Token::JMPGE) || match(Token::JMPLT) || match(Token::JMPLE)) { // mas casos
    tipo = 2;
    ttype = previous->type;
    
    if (!match(Token::ID)){
      cout << "Expecting jump label" << endl;
      exit(0);
    }

    jmplabel = previous->lexema;
    
  } else {
    cout << "Error: no pudo encontrar match para " << current << endl;
  }

 
  if (!match(Token::EOL)) {
    if (current->type != 5){
      cout << "esperaba fin de linea" << endl;
      exit(0);
    }
  }else{
    while (match(Token::EOL)){
      current = scanner->nextToken();
    }
  }

  if (tipo == 0) {
    instr = new Instruction(label, Token::tokenToIType(ttype));
  } else if (tipo == 2) {
    //instr = 
    instr = new Instruction(label, Token::tokenToIType(ttype), isint);
  } else { //
    //instr =
    instr = new Instruction(label, Token::tokenToIType(ttype), jmplabel);
  }
			   

  return instr;
}
```
* **Agregar todos los tokens (modificamos svm_parser.cpp->tokenToIType(Token::Type tt))**

```cpp
Instruction::IType Token::tokenToIType(Token::Type tt) {
  Instruction::IType itype;
  switch (tt) {
  case(Token::PUSH): itype = Instruction::IPUSH; break;
  case(Token::POP): itype = Instruction::IPOP; break;
  case(Token::DUP): itype = Instruction::IDUP; break;
  case(Token::ADD): itype = Instruction::IADD; break;
  case(Token::SUB): itype = Instruction::ISUB; break;
  case(Token::MUL): itype = Instruction::IMUL; break;
  case(Token::DIV): itype = Instruction::IDIV; break;
    // completar
  case(Token::GOTO): itype = Instruction::IGOTO; break;
  case(Token::JMPEQ): itype = Instruction::IJMPEQ; break;
  case(Token::JMPGT): itype = Instruction::IJMPGT; break;
  case(Token::JMPGE): itype = Instruction::IJMPGE; break;
  case(Token::JMPLT): itype = Instruction::IJMPLT; break;
  case(Token::JMPLE): itype = Instruction::IJMPLE; break;
  case(Token::SKIP): itype = Instruction::ISKIP; break;
  case(Token::STORE): itype = Instruction::ISTORE; break;
  case(Token::LOAD): itype = Instruction::ILOAD; break;
  case(Token::PRINT): itype = Instruction::IPRINT; break;
  default: cout << "Error: Unknown Keyword type" << endl; exit(0);
  }
  return itype;
}
```
* **Completamos factorial.svm**

```cpp
    //complete cod
```
