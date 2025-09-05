# Detecção do sistema (32 || 64 bits)
BITS_OPTION = -m64

# Definição do compilador (GCC++)
CPPC = g++

# Opções de compilação e includes
CCOPT = $(BITS_OPTION) -O3 -fPIC -fexceptions -DNDEBUG -DIL_STD -std=c++0x
CONCERTINCDIR = $(CONCERTDIR)/include
CCFLAGS = $(CCOPT)

# Diretórios dos códigos fontes e objetos (src e obj respectivamente)
SRCDIR = src 
OBJDIR = obj

# Lista de arquivos
SRCS = $(wildcard $(SRCDIR)/*.cpp)                           # Pega todos os .cpp dentro de src
OBJS = $(patsubst $(SRCDIR)/%.cpp, $(OBJDIR)/%.o, $(SRCS))   # Converte cada .cpp em .o correspondente dentro de obj

# Executável 
tsp: $(OBJS) # depende de todos os objetos ($(OBJS)), se algum objeto estiver desatualizado, ele recompila antes de linkar.
	@echo  "\033[31m \nLinking all objects files: \033[0m"
	$(CPPC) $(BITS_OPTION) $(OBJS) -o $@ $(CCLNFLAGS)
    # $(CPPC): comando de linkagem: gera o executável tsp
    # $@: nome do tsp
    # $(CCLNFLAGS): flags do linker 

# Inclusão de dependências
# Para cada obj, tenta incluir o .d correspondente (.d: dependências do headers)
-include $(OBJS:.o=.d)

# Regra para compilar objetos: compila o .cpp em .o e cria arquivo de dependências .d.
$(OBJDIR)/%.o: $(SRCDIR)/%.cpp
	@echo  "\033[31m \nCompiling $<: \033[0m"
	$(CPPC) $(CCFLAGS) -c $< -o $@
	@echo  "\033[32m \ncreating $< dependency file: \033[0m"
	$(CPPC) -std=c++0x  -MM $< > $(basename $@).d
	@mv -f $(basename $@).d $(basename $@).d.tmp #proximas tres linhas colocam o diretorio no arquivo de dependencias (g++ nao coloca, surprisingly!)
	@sed -e 's|.*:|$(basename $@).o:|' < $(basename $@).d.tmp > $(basename $@).d
	@rm -f $(basename $@).d.tmp

# Limpar arquivos gerados (Remove o executável tsp, .o e .d)
clean:
	@echo "\033[31mcleaning obj directory \033[0m"
	@rm tsp -f $(OBJDIR)/*.o $(OBJDIR)/*.d

# Recompila tudo
rebuild: clean tsp
