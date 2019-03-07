One Experience 
====================

Create One folder
----------------------

    mkdir ~/one
    cd ~/one
    

GIT config (nickname, e-mail)
-----------------------------

    git config --global user.email "mail@domain.com"
    git config --global user.name "login"
    

To initialize your local repository use
---------------------------------------

    repo init -u https://github.com/OneExperience/android.git -b android-8.1.0
    

Then to sync up:
----------------

    repo sync --current-branch --no-tags --no-clone-bundle --optimized-fetch --force-broken --force-sync -j16

Adding support for new device
================

If you want to create OneExperience Rom for your Device,then you to create these two files in device tree:

one_device.mk sample
----------

    # Inherit from those products. Most specific first.
    $(call inherit-product, $(SRC_TARGET_DIR)/product/core_64_bit.mk) -- only for 64bit phones
    $(call inherit-product, $(SRC_TARGET_DIR)/product/full_base_telephony.mk)

    # Inherit from device
    $(call inherit-product, device/<path>/device.mk) -- path to main device makefile

    # Inherit common product files.
    $(call inherit-product, vendor/one/config/common_full_phone.mk)

    # Set those variables here to overwrite the inherited values.
    BOARD_VENDOR := 
    PRODUCT_BRAND := 
    PRODUCT_DEVICE := 
    PRODUCT_NAME := one_device
    PRODUCT_MANUFACTURER := 
    PRODUCT_MODEL := 
    TARGET_VENDOR := 

one.dependencies sample
----------

    [
      {
        "repository": "android_kernel_<name>",
        "target_path": "kernel/path"
      },
      {
        "repository": "proprietary_vendor_<name>",
        "target_path": "vendor/path"
      },
      {
        "repository": "android_device_<name>-common"
        "target_path": "device/path"
      }
    ]


Build command is
----------------
    export JACK_SERVER_VM_ARGUMENTS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx4000m"
    . build/envsetup.sh
    lunch one_$device-userdebug
    make -jx otapackage

