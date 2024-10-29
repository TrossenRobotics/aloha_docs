SOURCE  = .
OUT     = _build
BUILD   = python3 -m sphinx
OPTS    =
LATEST  = 2.0

help:
	@$(BUILD) -M help "$(SOURCE)" "$(OUT)" $(OPTS) $(O)
	@echo "  multiversion to build documentation for all branches"

multiversion: Makefile
	sphinx-multiversion $(OPTS) "$(SOURCE)" "$(OUT)/html"
	@echo "<html><head><meta http-equiv=\"refresh\" content=\"0; url=$(LATEST)/index.html\" /></head></html>" > $(OUT)/html/index.html

.PHONY: help Makefile multiversion

%: Makefile
	@$(BUILD) -M $@ "$(SOURCE)" "$(OUT)" $(OPTS) $(O)
