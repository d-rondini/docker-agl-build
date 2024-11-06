AGL_MACHINE ?= ebisu
FEATURE := agl-demo
RECIPE := agl-ivi-demo-qt
BRANCH := quillback

all: fetch configure build

# Fetch updates
fetch:
	$(info Fetching...)
	(repo init -b ${BRANCH} -u  https://gerrit.automotivelinux.org/gerrit/AGL/AGL-repo && repo sync)

# Clean configuration
clean-configuration:
	$(info Cleaning previous configuration...)
	rm -rf build/conf build/agl*

# Create build configuration
configure: clean-configuration
	$(info Creating configuration...)
	@bash -c "source meta-agl/scripts/aglsetup.sh -m ${AGL_MACHINE} ${FEATURE}"

.PHONY: build

# Build image
build:
	$(info Building RECIPE = ${RECIPE})
	@bash -c "cd build && \
	source ./agl-init-build-env && \
	bitbake ${RECIPE}"

clean: clean-configuration
	rm -rf .repo* meta* bsp external
