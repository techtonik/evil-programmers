OBJDIR = ../o
REOBJDIR = \.\.\/o\/
DLLDIR = ../bin
DLLNAME = EditCase.dll
DLLFULLNAME = $(DLLDIR)/$(DLLNAME)
SRCS = EditCase.cpp memory.cpp
DEF = EditCase.gcc.def

CXX = g++
DLLTOOL = dlltool
RM = rm -f
CP = cp -f
M4 = m4 -P
MV = mv -f
MKDIR = mkdir -p
CXXFLAGS = -Wall -Os -funsigned-char -fomit-frame-pointer -fstrict-aliasing -fno-rtti -fno-exceptions
LNKFLAGS = -mdll -s -nostartfiles -lcrtdll

all: $(DLLFULLNAME)

OBJS = $(patsubst %.cpp,$(OBJDIR)/%.o,$(filter %.cpp,$(SRCS)))
DEPS = $(patsubst %.cpp,$(OBJDIR)/%.d,$(filter %.cpp,$(SRCS)))
MAP = $(patsubst %.dll,%.map,$(DLLFULLNAME))

$(OBJDIR)/%.d: %.cpp
	@echo making depends for $<
	@$(SHELL) -ec '$(CXX) -c -MM $(CXXFLAGS) $< \
                | sed '\''s/\($*\)\.o[ :]*/$(REOBJDIR)\1.o $(REOBJDIR)\1.d: /g'\'' > $@; [ -s $@ ] || $(RM) $@'

$(OBJDIR)/%.o: %.cpp
	@echo compiling $<
	@$(CXX) $(CXXFLAGS) -c -o $@ $<

$(DLLFULLNAME): $(OBJS) $(DEF)
	@echo linking $@
	@$(CXX) -mdll -o $(DLLNAME) -Xlinker --base-file -Xlinker $(DLLNAME).base $(OBJS) $(LNKFLAGS)
	@$(DLLTOOL) --dllname $(DLLNAME) --base-file $(DLLNAME).base --output-exp $(DLLNAME).exp --def $(DEF)
	@$(CXX) -mdll  -o $(DLLNAME) $(OBJS) $(DLLNAME).exp $(LNKFLAGS)
#-Xlinker -Map -Xlinker $(MAP)
	@$(MV) $(DLLNAME) $(DLLDIR)
	@$(RM) $(DLLNAME).base
	@$(RM) $(DLLNAME).exp

-include $(DEPS)
