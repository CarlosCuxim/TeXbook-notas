CONVERT=dvisvgm
FLAGS=--pdf

%.svg: %.pdf
	$(CONVERT) $(FLAGS) $<
	make clean-tex
	make clean-pdf

%.pdf: %.tex
	latexmk -pdf -interaction=nonstopmode $<

clean-tex:
	rm -f *.aux *.fdb_latexmk *.fls *.log *.synctex.gz

clean-pdf:
	rm -f *.pdf

clean-svg:
	rm -f *.svg