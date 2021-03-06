# Copyright Jetstack Ltd. See LICENSE for details.
# Minimal makefile for Sphinx documentation
#

# You can set these variables from the command line.
VENV_PATH    ?= venv
SPHINXOPTS    = -W
SPHINXBUILD   = $(VENV_PATH)/bin/python -msphinx
SPHINXPROJ    = Tarmak
SOURCEDIR     = .
BUILDDIR      = _build

# Put it first so that "make" without argument is like "make help".
help:
	make " help"

.PHONY: help Makefile clean

# Catch-all target: route all unknown targets to Sphinx using the new
# "make mode" option.  $(O) is meant as a shortcut for $(SPHINXOPTS).
%: Makefile .venv
	@$(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

.venv:
	virtualenv -p $(shell which python3) $(VENV_PATH)
	$(VENV_PATH)/bin/pip install -r requirements.txt
	touch .venv

clean:
	rm -rf .venv "$(BUILDDIR)"

copy_artifacts:
	mkdir -p ../_artifacts
	cp -a $(BUILDDIR)/html ../_artifacts/docs

multiversion_build:
	$(VENV_PATH)/bin/sphinx-versioning build -r master -w "^(master|release-\d+\.\d+)$$" -W "none" ./docs ./_build

multiversion_push:
	gsutil -m rsync -rd _build/ gs://docs.tarmak.io/
