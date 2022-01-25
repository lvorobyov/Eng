TARGET = barkalov.pdf
TEX = pdftex
LATEX ?= pdflatex
ADOCS = $(filter-out barkalov.adoc,$(wildcard *.adoc))
LATEXOPT = -draftmode -shell-escape

all: ${TARGET}

%.aux: %.tex preamble.fmt
	${LATEX} -draftmode -shell-escape "&preamble $<"

%.pdf: %.tex $(ADOCS:.adoc=.tex) preamble.fmt
	${LATEX} -shell-escape "&preamble $<"

%.fmt: %.tex
	${TEX} -shell-escape -ini -jobname="preamble" "&${LATEX} $^\dump" "$<"

barkalov.tex: barkalov.adoc 
	ad2tex.pl $< > $@.bak
	perl -lpe 'BEGIN{print "\\begin{document}"} END{print "\n\\end{document}"}' < $@.bak > $@
	rm $@.bak

%.tex: %.adoc
	ad2tex.pl $< > $@

