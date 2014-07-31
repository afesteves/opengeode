all: test-ada

edit:
	../../../opengeode.py myfunction.pr system_structure.pr

check:
	../../../opengeode.py myfunction.pr system_structure.pr --check

test-parse:
	../../../opengeode.py myfunction.pr system_structure.pr --check

test-ada:
	../../../opengeode.py --toAda myfunction.pr system_structure.pr || exit 1
	asn1.exe -Ada dataview-uniq.asn -typePrefix asn1Scc -equal
	asn1.exe -c dataview-uniq.asn -typePrefix asn1Scc
	gnatmake -c myfunction.adb
	gcc -c test.c
	gnatbind -n myfunction.ali
	gnatlink -o testcase test.o myfunction.ali -lgnat
	./testcase | diff expected -

coverage:
	coverage run  -p ../../../opengeode.py myfunction.pr system_structure.pr --toAda


clean:
	rm -rf *.adb *.ads *.pyc runSpark.sh spark.idx *.o *.ali gnat.cfg examiner bin *.wrn *.gpr datav*.? ber.c xer.c asn1crt.? acn.c real.c testcase