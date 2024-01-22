

FILES=dosscr dosmkdata dosvfdata dosi0 doscat dosdir dosval dosdir dostst dosfmt

all: $(FILES)

%.bin: %.a65
	xa -XMASM -o $@ $<

upet-fiec-core.bin: upet-fiec-core.a65
	xa -P $@.lst -DPET -XMASM -o $@ $<

%: %.a65 common.a65 upet-fiec-core.bin
	xa -XMASM -w -P $@.lst -DPET -o $@ $<
	
clean:
	rm -f *.bin *.lst
	rm -f $(FILES)

