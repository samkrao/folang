## 📘 Roadmap (Milestone Checklist)
- 🟩 Complete
- 🟨 In progress
- ⬜ Not started


### **Phase 1 — Foundations**

- 🟩 Define core philosophy  
- 🟩 Repo structure and documentation  
- 🟩 Architectural overview  
- 🟩 Initial design notes  
- 🟩 Feature freeze for v0.1
- 🟩 Type system + shape semantics draft  
- 🟩 Early internal experiments  

### **Phase 2 — Frontend (Parsing & Analysis)**
- 🟩 Tokenizer / lexer  
- 🟩 Complete grammar  
- 🟩 AST node definitions  
- 🟨 Parse all constructs  
- 🟨 Syntax validation  
- 🟨 Semantic analysis  
  - 🟨 Scopes  
  - 🟨 Name resolution  
  - 🟨 Basic type checks  
- 🟨 Full AST → JSON  
- 🟨 Error messages  
- 🟨 Grammar / AST tests  

### **Phase 3 — Backend (C++ + GCC)**  
- 🟨 AST → C++ IR  
- ⬜ Minimal valid C++ generation  
- ⬜ Integer expressions  
- ⬜ Variables  
- ⬜ Simple functions  
- ⬜ Branching  
- ⬜ Minimal runtime  
- ⬜ End-to-end: `fo → C++ → GCC → executable`  
- ⬜ Expand codegen coverage  

### **Phase 4 — Bug Fixes & Stability**
- ⬜ Parser fixes  
- ⬜ Type resolution fixes  
- ⬜ Codegen fixes  
- ⬜ Regression tests  
- ⬜ Better diagnostics  
- ⬜ Documentation updates  

### **Phase 5 — Clang Support**
- ⬜ Backend abstraction  
- ⬜ Clang compatibility  
- ⬜ Consistency with GCC backend  
- ⬜ Clang-specific notes  
- ⬜ End-to-end validation  
