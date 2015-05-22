# specify your main target here:
all: book pod cover

# specify thh main file and all the files that you are including
SOURCE= main.tex 01-einfuehrung.tex 02-valenz.tex 03-kohaerenz.tex 04-ellipse.tex 05-tag.tex 06-tag-kohaerenz.tex 07-ttmctag-kohaerenz.tex 08-tag-ellipse.tex 09-syntax-ohne-valenz.tex 10-schluss.tex \
acronyme.tex \
literatur.bib \
LSP/langsci.cls
	 
main.pdf: main.tex $(SOURCE)
	xelatex -no-pdf main 
	bibtex -min-crossrefs=200 main
	xelatex  -no-pdf main
	sed -i s/.*\\emph.*// main.adx #remove titles which biblatex puts into the name index
	makeindex -o main.and main.adx
	makeindex -o main.lnd main.ldx
	makeindex -o main.snd main.sdx
	xelatex -no-pdf main 
	xelatex main

index: main.adx main.ldx main.sdx
	sed -i s/.*\\emph.*// main.adx #remove titles which biblatex puts into the name index
	makeindex -o main.and main.adx
	makeindex -o main.lnd main.ldx
	makeindex -o main.snd main.sdx

#create only the book
book: main.pdf 

#create a png of the cover
cover: main.pdf
	convert main.pdf\[0\] -quality 100 -background white -alpha remove -bordercolor black -border 2  cover.png
	display cover.png

#extract the front cover, back cover and spine from the pdf	
triptychon: main.pdf#output=long has to be used for this to work
	pdftk A=main.pdf cat  A1  output front.pdf 
	pdftk A=main.pdf cat A2 output back.pdf 
	pdftk A=main.pdf cat A3 output spine.pdf

#prepare for print on demand services	
pod: bod createspace

#prepare for submission to BOD
bod: triptychon #output=long has to be used for this to work
	pdftk A=main.pdf B=blank.pdf cat B A4-end output tmp.pdf 
	./filluppages tmp.pdf bod/bodcontent.pdf
	\rm tmp.pdf
	xelatex bodcover.tex
	mv bodcover.pdf bod/

# prepare for submission to createspace
createspace: triptychon #output=long has to be used for this to work
	xelatex createspacecover.tex
	mv createspacecover.pdf createspace
	pdftk A=main.pdf B=blank.pdf cat B A4-end output createspace/createspacecontent.pdf

#housekeeping	
clean:
	rm -f *.bak *~ *.backup *.tmp \
	*.adx *.and *.idx *.ind *.ldx *.lnd *.sdx *.snd *.rdx *.rnd *.wdx *.wnd \
	*.log *.blg *.ilg \
	*.aux *.toc *.cut *.out *.tpm *.bbl *-blx.bib *_tmp.bib \
	*.glg *.glo *.gls *.wrd *.wdv *.xdv \
	*.run.xml

realclean: clean
	rm -f *.dvi *.ps *.pdf 
