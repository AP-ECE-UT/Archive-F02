include common_vars.mk

EXE_SPUTIFY   := sputify.out

VPATH = $(PATH_SRC)

FILES   = $(patsubst src/%, %, $(shell find $(PATH_SRC) -name "*.cpp" -type f))
FOLDERS = $(patsubst src/%, %, $(shell find $(PATH_SRC) -mindepth 1 -type d))

FILES_NOMAIN = $(filter-out main.cpp, $(FILES))

FILES_DEP = $(patsubst %, $(PATH_DEP)/%.d, $(basename $(FILES)))
FILES_OBJ = $(patsubst %, $(PATH_OBJ)/%.o, $(basename $(FILES_NOMAIN)))

#----------------------------------------

all: $(PATH_BIN)/$(EXE_SPUTIFY) 

$(PATH_BIN)/$(EXE_SPUTIFY): $(PATH_OBJ)/main.o $(FILES_OBJ)
	$(CC) $(LDFLAGS) $^ $(LDLIBS) -o $@

DEPFLAGS    = -MT $@ -MMD -MP -MF $(PATH_DEP)/$*.dTMP
POSTCOMPILE = @$(MOVE) $(PATH_DEP)/$*.dTMP $(PATH_DEP)/$*.d > $(NULL_DEVICE) && touch $@

$(PATH_OBJ)/%.o: %.cpp
$(PATH_OBJ)/%.o: %.cpp $(PATH_DEP)/%.d | directories
	$(CC) $(CPPFLAGS) -c $(DEPFLAGS) $< -o $@
	$(POSTCOMPILE)

.PRECIOUS: $(FILES_DEP)
$(FILES_DEP): ;
-include $(FILES_DEP)

#----------------------------------------

directories: $(PATH_BUILD) $(PATH_BIN) $(PATH_OBJ) $(PATH_DEP) nested-folders
nested-folders: $(addprefix $(PATH_OBJ)/, $(FOLDERS)) $(addprefix $(PATH_DEP)/, $(FOLDERS))

$(PATH_BUILD): ; $(MKDIR) $@
$(PATH_BIN)  : ; $(MKDIR) $@
$(PATH_OBJ)  : ; $(MKDIR) $@
$(PATH_DEP)  : ; $(MKDIR) $@

$(addprefix $(PATH_OBJ)/, $(FOLDERS)): ; @$(MKDIR) $@
$(addprefix $(PATH_DEP)/, $(FOLDERS)): ; @$(MKDIR) $@

#----------------------------------------

.PHONY: all directories nested-folders \
		clean clean-obj clean-dep clean-exe \
		run help

clean: clean-obj clean-dep clean-exe clean-fifos clean-logs
clean-obj   : ; $(RMDIR) $(PATH_OBJ)/*
clean-dep   : ; $(RMDIR) $(PATH_DEP)/* 
clean-fifos : ; $(RMDIR) $(PATH_BUILD)/fifos/* 
clean-logs  : ; $(RMDIR) $(PATH_BUILD)/logs/*
clean-exe   : ; $(RMDIR) $(PATH_BIN)/$(EXE_SPUTIFY)

ARGS ?=
run: ; @cd $(PATH_BIN) && ./$(EXE_SPUTIFY) 

help:
	@echo Targets: all clean clean-obj clean-dep clean-exe delete-build run
	@echo '(make run ARGS="arg1 arg2...")'
