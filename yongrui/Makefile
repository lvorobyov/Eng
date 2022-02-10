# GNU makefile for conversion AsciiDoc to LaTeX, PDF and DocX
# Copyright (c) 2022, Lev O. Vorobyov
TARGET = yongrui.pdf
TEX = xetex
LATEX ?= xelatex
LATEXOPT = -shell-escape
ADOCOPTS = -a lang=ru
DBSTYLE = gost-article
TMPDIR = texfiles
LATEXOPT += --output-directory=./${TMPDIR}
EVAL = $(shell sed -ne '/TEXINPUTS=.*/ p' ${TMPDIR}/env_tex)
BIBFILE = $(wildcard *.bib)

all: ${TARGET}

${TMPDIR}/%.aux: %.tex
	${EVAL} ${LATEX} ${LATEXOPT} ${OUTDIR} $<

%.pdf: %.tex ${TMPDIR}/%.aux
	${EVAL} ${LATEX} ${LATEXOPT} ${OUTDIR} $<
	mv ${TMPDIR}/$@ .

%.tex: %.adoc $(filter-out %.adoc, $(wildcard *.adoc)) ${BIBFILE}
	asciidoctor -b docbook5 -o - $< -r asciidoctor-bibtex ${ADOCOPTS} | \
	dblatex -ttex -T ${DBSTYLE} -d --tmpdir=./${TMPDIR} -o $@ -

%.docx: %.adoc ${BIBFILE} styles.docx
	asciidoctor -b docbook5 -o - $< -r asciidoctor-bibtex ${ADOCOPTS} | \
	pandoc -f docbook -t docx -o $@ --reference-doc=${DBSTYLE}.docx

%.html: %.adoc ${BIBFILE}
	asciidoctor -r asciidoctor-bibtex ${ADOCOPTS} $<

clean:
	rm -rf ${TMPDIR}
