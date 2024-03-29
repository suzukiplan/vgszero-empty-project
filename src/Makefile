PROJECT = mygame
SRC_FILES = main.rel table.rel
CHR_FILES = image1.chr
WAV_FILES = se1.wav se2.wav
BGM_FILES = song1.bgm
VGSHOME = ../vgszero
BMP2CHR = ${VGSHOME}/tools/bmp2chr/bmp2chr
VGSMML = ${VGSHOME}/tools/vgsmml/vgsmml
MAKEROM = ${VGSHOME}/tools/makerom/makerom
MAKEBGM = ${VGSHOME}/tools/makebgm/makebgm
MAKESE = ${VGSHOME}/tools/makese/makese
MAKEPKG = ${VGSHOME}/tools/makepkg/makepkg
COMMON_FLAGS = -mz80
COMMON_FLAGS += --opt-code-size
COMMON_FLAGS += --code-loc 0x100
COMMON_FLAGS += --xram-loc 0xC000 
COMMON_FLAGS += --xram-size 0x4000 
COMMON_FLAGS += --no-std-crt0
COMPILE = sdcc
COMPILE += ${COMMON_FLAGS}
COMPILE += -c
LINK = sdcc
LINK += ${COMMON_FLAGS}
LINK += -Wl${VGSHOME}/lib/sdcc/vgs0.lib
LINK += -o program.ihx
LINK += ${SRC_FILES}

all: build
	cd ${VGSHOME}/src/sdl2 && make
	${VGSHOME}/src/sdl2/vgs0 game.pkg

build: game.pkg

clean:
	rm -f *.rel
	rm -f *.chr
	rm -f *.lst
	rm -f *.sym
	rm -f *.asm
	rm -f *.ihx
	rm -f *.map
	rm -f *.bin
	rm -f *.lk
	rm -f *.noi
	rm -f *.bgm
	rm -f ${PROJECT}.rom
	rm -f bgm.dat
	rm -f se.dat
	rm -f game.pkg

game.pkg: ${PROJECT}.rom bgm.dat se.dat
	${MAKEPKG} -o game.pkg -r ${PROJECT}.rom -b bgm.dat -s se.dat

${PROJECT}.rom: program.bin ${CHR_FILES} palette.bin
	${MAKEROM} ${PROJECT}.rom program.bin ${CHR_FILES} palette.bin

program.bin: ${SRC_FILES} ${VGSHOME}/lib/sdcc/vgs0.lib ${VGSHOME}/lib/sdcc/vgs0lib.h
	${LINK}
	makebin -s 32768 program.ihx $@

se.dat: ${WAV_FILES}
	${MAKESE} $@ ${WAV_FILES}

bgm.dat: ${BGM_FILES}
	${MAKEBGM} $@ ${BGM_FILES}

palette.bin: image1.bmp
		${BMP2CHR} $< image1.chr palette.bin

.SUFFIXES: .rel .src

%.rel: %.c header.h
	${COMPILE} $<

.SUFFIXES: .rel .c

%.rel: %.c header.h
	${COMPILE} $<

.SUFFIXES: .chr .bmp

%.chr: %.bmp
	${BMP2CHR} $< $@

.SUFFIXES: .bgm .mml

%.bgm: %.mml
	${VGSMML} $< $@
