include ../shared.mk

all: test-ada test-llvm

edit:
	$(OPENGEODE) orchestrator.pr system_structure.pr

test-parse:
	$(OPENGEODE) orchestrator.pr system_structure.pr --check

test-ada: 
	$(CC) -c -g test_ada.c
	$(OPENGEODE) orchestrator.pr system_structure.pr --shared
	$(ASN1SCC) -Ada dataview-uniq.asn -typePrefix asn1Scc -equal
	$(ASN1SCC) -c dataview-uniq.asn -typePrefix asn1Scc -equal
	$(GNATMAKE) -O$(O) -gnat2012 -c *.adb
	$(GNATBIND) -n orchestrator.ali
	$(GNATLINK) -o test_ada test_ada.o orchestrator.ali -lgnat
	./test_ada

test-llvm: orchestrator.o

test-python:
	$(OPENGEODE) orchestrator.pr system_structure.pr --shared
	$(ASN1SCC) -Ada dataview-uniq.asn -typePrefix asn1Scc -equal
	$(ASN1SCC) -c dataview-uniq.asn -typePrefix asn1Scc -equal
	$(GNATMAKE) -O$(O) -gnat2012 -c *.adb
	$(GNATBIND) -n -Lliborchestrator orchestrator
	$(GNATMAKE) -c -gnat2012 b~orchestrator.adb
	$(CC) -c external_proc.c
	$(CC) -shared -fPIC -o liborchestrator.so b~orchestrator.o orchestrator.o taste_basictypes.o adaasn1rtl.o external_proc.o -lgnat
	python -c "from ctypes import *; test=CDLL('./liborchestrator.so'); test.liborchestratorinit()"

coverage:
	coverage run -p $(OPENGEODE) orchestrator.pr system_structure.pr --toAda

.PHONY: all edit test-parse test-ada test-llvm coverage