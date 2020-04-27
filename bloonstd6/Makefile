# If you want to compile for arm64e, you'll need a macOS device or a arm64e device that's able to compile for arm64e.
# Once done that, uncomment the "#arm64e" by removing the "#"

THEOS_DEVICE_IP = 127.0.0.1 -p 2222 # install to device from pc
ARCHS = arm64 arm64e
DEBUG = 0
FINALPACKAGE = 1
FOR_RELEASE = 1

# 0 to treat warnings as errors, 1 otherwise.
IGNORE_WARNINGS=0

# only set this  to 1 if you are on mobile theos
# assuming you have an sdk at your theos sdks directory
# this will include c++ headers and other needed headers for your project so you don't need to manually include them or something like that
# if some c++ headers are still missing in your sdk like "initializer_list" then manually copy them to your c++ headers directory and not your project folder
# for example in my case c++ headers directory is located at /private/var/theos/sdks/iPhoneOS11.2.sdk/usr/include/c++/4.2.1/
# please note, do not include c++ headers in your theos includes to enable c++ which is a ghetto solution and use this approach instead
MOBILE_THEOS=0
ifeq ($(MOBILE_THEOS),1)
  # path to your sdk
  SDK_PATH = $(THEOS)/sdks/iPhoneOS11.2.sdk/
  $(info ===> Setting SYSROOT to $(SDK_PATH)...)
  SYSROOT = $(SDK_PATH)
else
  TARGET = iphone:clang:latest:8.0
endif


## Common frameworks ##
PROJ_COMMON_FRAMEWORKS = UIKit Foundation Security QuartzCore CoreGraphics CoreText

## source files ##
KITTYMEMORY_SRC = $(wildcard KittyMemory/*.cpp)
SCLALERTVIEW_SRC =  $(wildcard SCLAlertView/*.m)
MENU_SRC = Menu.mm

include $(THEOS)/makefiles/common.mk

TWEAK_NAME = bloonstd6

bloonstd6_CFLAGS = -fobjc-arc
bloonstd6_CCFLAGS = -std=c++11 -fno-rtti -fno-exceptions -DNDEBUG

ifeq ($(IGNORE_WARNINGS),1)
  bloonstd6_CFLAGS += -w
  bloonstd6_CCFLAGS += -w
endif


bloonstd6_FILES = Tweak.xm $(MENU_SRC) $(KITTYMEMORY_SRC) $(SCLALERTVIEW_SRC)

bloonstd6_LIBRARIES += substrate

bloonstd6_FRAMEWORKS = $(PROJ_COMMON_FRAMEWORKS)
# GO_EASY_ON_ME = 1

include $(THEOS_MAKE_PATH)/tweak.mk

internal-package-check::
	@./versionCheck.sh # Script to verify template's current version

after-install::
	install.exec "killall -9 bloonstd6 || :"