# Specify the IP address and port for Theos to install the tweak to the device
THEOS_DEVICE_IP = 127.0.0.1
THEOS_DEVICE_PORT = 2222

# Specify the architectures to compile for
ARCHS = arm64 arm64e

# Set debug and release options
DEBUG = 0
FINALPACKAGE = 1
FOR_RELEASE = 1

# Set to 0 to treat warnings as errors, 1 otherwise
IGNORE_WARNINGS = 0

# Set this to 1 if you are using mobile Theos
# Ensure you have an SDK in your Theos SDK directory
# This includes necessary headers for your project
MOBILE_THEOS = 0
ifeq ($(MOBILE_THEOS), 1)
  # Path to your SDK
  SDK_PATH = $(THEOS)/sdks/iPhoneOS11.2.sdk/
  $(info ===> Setting SYSROOT to $(SDK_PATH)...)
  SYSROOT = $(SDK_PATH)
else
  TARGET = iphone:clang:latest:8.0
endif

# Common frameworks used in the project
PROJ_COMMON_FRAMEWORKS = UIKit Foundation Security QuartzCore CoreGraphics CoreText

# Source files
KITTYMEMORY_SRC = $(wildcard KittyMemory/*.cpp)
SCLALERTVIEW_SRC = $(wildcard SCLAlertView/*.m)
MENU_SRC = Menu.mm

# Include common Theos makefiles
include $(THEOS)/makefiles/common.mk

# Name of the tweak
TWEAK_NAME = bloonstd6

# Compiler flags
bloonstd6_CFLAGS = -fobjc-arc
bloonstd6_CCFLAGS = -std=c++11 -fno-rtti -fno-exceptions -DNDEBUG

# Handle warnings based on IGNORE_WARNINGS setting
ifeq ($(IGNORE_WARNINGS), 1)
  bloonstd6_CFLAGS += -w
  bloonstd6_CCFLAGS += -w
endif

# Files to be compiled
bloonstd6_FILES = Tweak.xm $(MENU_SRC) $(KITTYMEMORY_SRC) $(SCLALERTVIEW_SRC)

# Libraries to link against
bloonstd6_LIBRARIES += substrate

# Frameworks to link against
bloonstd6_FRAMEWORKS = $(PROJ_COMMON_FRAMEWORKS)

# Include the tweak makefile
include $(THEOS_MAKE_PATH)/tweak.mk

# Internal package check script
internal-package-check::
	@./versionCheck.sh # Script to verify template's current version

# Post-installation actions
after-install::
	install.exec "killall -9 bloonstd6 || :"
