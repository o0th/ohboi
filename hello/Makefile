# Makefile for building Game Boy projects with RGBDS


# Source files
SOURCES = main.asm

# Object files
OBJECTS = hello.o

# Target output file
TARGET = hello.gb

# Default rule
all: $(TARGET)

# Rule for building the final ROM
$(TARGET): $(OBJECTS)
	rgblink -o $@ $^
	rgbfix -v -p 0xFF $@

# Rule for assembling the source files
$(OBJECTS): $(SOURCES)
	rgbasm -L -o $@ $^

# Clean rule to remove generated files
clean:
	rm -f $(OBJECTS) $(TARGET)

.PHONY: all clean
