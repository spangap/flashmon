# Build the peripheral-detection RAM app and stage its image where the browser
# serves it: ../flashmon/detect/spangap_detect.bin for the ESP32-S3, and
# ../flashmon/detect/spangap_detect_<target>.bin for any other chip (those ARE
# checked in — the build directories are not). A detector is one chip's RAM
# image and holds that chip's boards only; flashmon picks it by the chip the ROM
# reports.
#
#   make                     the ESP32-S3 detector, built in the spangap container
#   make TARGET=esp32p4      the ESP32-P4 detector, likewise
#   make LOCAL=1 [TARGET=…]  build with an ESP-IDF already activated on your PATH
#   make clean               remove every build directory
#
# The container build uses `spangap detect-build`, which runs idf.py inside the
# workspace's build container — so all you need is a spangap workspace with this
# repo checked out.

TARGET ?= esp32s3

ifeq ($(TARGET),esp32s3)
BDIR := build
DEST := ../flashmon/detect/spangap_detect.bin
IDFARGS := set-target esp32s3 build
else
BDIR := build.$(TARGET)
DEST := ../flashmon/detect/spangap_detect_$(TARGET).bin
IDFARGS := -B $(BDIR) -DSDKCONFIG=$(BDIR)/sdkconfig set-target $(TARGET) build
endif
BIN := $(BDIR)/spangap_detect.bin

.PHONY: all build clean
all: build
	cp $(BIN) $(DEST)
	@echo "staged detector -> $(DEST)"

build:
ifeq ($(LOCAL),1)
	idf.py $(IDFARGS)
else
	spangap detect-build $(TARGET)
endif

clean:
	rm -rf build build.*
