# You can set these variables from the command line, and also
# from the environment for the first two.
SPHINXOPTS    ?=
SPHINXBUILD   ?= sphinx-build
SOURCEDIR     = .
BUILDDIR      = _build
TRANSITIONS   = $(sort $(patsubst $(SOURCEDIR)/transition/%/index.rst,%,$(wildcard $(SOURCEDIR)/transition/*/index.rst)))

.PHONY: Makefile publish transition clean $(TRANSITIONS)

sphinx-static/switchers.js.in:
	git checkout master sphinx-static/switchers.js.in

set_versions.py:
	git checkout master set_versions.py

transition: $(TRANSITIONS)

$(TRANSITIONS): sphinx-static/switchers.js.in set_versions.py
	./set_versions.py $@
	@$(SPHINXBUILD) -M html "$(SOURCEDIR)"/transition/$@/ "$(BUILDDIR)"/$@ $(SPHINXOPTS) $(O) -c "$(SOURCEDIR)"

publish: transition
	rm -rf $(BUILDDIR)/final/
	mkdir -p $(BUILDDIR)/final/
	for ver in $(TRANSITIONS) ; do \
		mkdir -p $(BUILDDIR)/final/$$ver ;\
		cp -r $(BUILDDIR)/$$ver/html/* $(BUILDDIR)/final/$$ver/ ;\
	done

clean:
	@rm -rf $(BUILDDIR)
