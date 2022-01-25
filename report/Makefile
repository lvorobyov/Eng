TARGET = report.pdf
TEX = pdftex
LATEX ?= pdflatex
ADOCS = $(filter-out report.adoc,$(wildcard *.adoc)) glossary.adoc
LATEXOPT = -draftmode -shell-escape

all: ${TARGET}

%.aux: %.tex preamble.fmt
	${LATEX} -draftmode -shell-escape "&preamble $<"

%.bcf: %.aux links.bib
	biber $(basename $<)

%.pdf: %.tex $(ADOCS:.adoc=.tex) preamble.fmt %.bcf
	${LATEX} -shell-escape "&preamble $<"
	${LATEX} -shell-escape "&preamble $<"

%.fmt: %.tex
	${TEX} -shell-escape -ini -jobname="preamble" "&${LATEX} $^\dump" "$<"

report.tex: report.adoc links.bib
	ad2tex.pl $< > $@.bak
	perl -lpe 'BEGIN{print "\\begin{document}"} END{print "\n\\end{document}"}' < $@.bak > $@
	rm $@.bak

%.tex: %.adoc
	ad2tex.pl $< > $@

glossary.adoc: glossary.tt2 glossary.csv
	tpage --post_chomp $< > $@

