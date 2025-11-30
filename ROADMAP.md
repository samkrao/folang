## 📘 Roadmap (Milestone Checklist)

### **Phase 1 — Foundations**
- [x] Define core philosophy  
- [x] Repo structure and documentation  
- [x] Architectural overview  
- [x] Initial design notes  
- [x] Type system + shape semantics draft  
- [x] Early internal experiments  

### **Phase 2 — Frontend (Parsing & Analysis)**
- [x] Tokenizer / lexer  
- [x] Complete grammar  
- [x] AST node definitions  
- [-] Parse all constructs  
- [-] Syntax validation  
- [-] Semantic analysis  
  - [-] Scopes  
  - [-] Name resolution  
  - [-] Basic type checks  
- [-] Full AST → JSON  
- [-] Error messages  
- [-] Grammar / AST tests  

### **Phase 3 — Backend (C++ + GCC)**  
- [-] AST → C++ IR  
- [ ] Minimal valid C++ generation  
- [ ] Integer expressions  
- [ ] Variables  
- [ ] Simple functions  
- [ ] Branching  
- [ ] Minimal runtime  
- [ ] End-to-end: `fo → C++ → GCC → executable`  
- [ ] Expand codegen coverage  

### **Phase 4 — Bug Fixes & Stability**
- [ ] Parser fixes  
- [ ] Type resolution fixes  
- [ ] Codegen fixes  
- [ ] Regression tests  
- [ ] Better diagnostics  
- [ ] Documentation updates  

### **Phase 5 — Clang Support**
- [ ] Backend abstraction  
- [ ] Clang compatibility  
- [ ] Consistency with GCC backend  
- [ ] Clang-specific notes  
- [ ] End-to-end validation  
