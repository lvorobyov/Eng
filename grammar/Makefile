TARGET = report.pdf
TEX = pdftex
LATEX ?= pdflatex
ADOCS = $(wildcard *.adoc)
LATEXOPT = -draftmode -shell-escape

all: ${TARGET}

%.aux: %.tex preamble.fmt
	${LATEX} -draftmode -shell-escape "&preamble $<"

%.bcf: %.aux links.bib
	biber $(basename $<)

%.pdf: %.tex intro.tex concl.tex glossary.tex unit1.tex unit6.tex threadsafety.tex flashloan.tex barkalov.tex research.tex preamble.fmt %.bcf
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

%.bib: %.txt
	txt2bib.pl $<
	tpage --eval_perl -post_chomp /usr/bin/site_perl/bibtex.tt > $@

