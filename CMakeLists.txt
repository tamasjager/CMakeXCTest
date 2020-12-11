cmake_minimum_required(VERSION 3.18)

set(CMAKE_OSX_DEPLOYMENT_TARGET "13.4" CACHE STRING "min")
set(CMAKE_XCODE_ATTRIBUTE_TARGETED_DEVICE_FAMILY "1,2")

project(CMake_XCTest)

# Test host target
set(HOST_NAME "TestHost")

add_executable(${HOST_NAME} MACOSX_BUNDLE)
target_sources(${HOST_NAME} PRIVATE "dummy_testHost.m")

set_target_properties(${HOST_NAME} PROPERTIES
    MACOSX_BUNDLE_INFO_PLIST "Xcode_TestHostTarget-Info.plist.in"
    XCODE_ATTRIBUTE_PRODUCT_NAME "${HOST_NAME}"
    XCODE_ATTRIBUTE_PRODUCT_BUNDLE_IDENTIFIER "com.${HOST_NAME}"
)

# Tests target
set(TESTS_NAME "Tests")

find_package("XCTest" REQUIRED)
xctest_add_bundle(${TESTS_NAME} ${HOST_NAME})
xctest_add_test(XCTest.TestHost ${TESTS_NAME})
target_sources(${TESTS_NAME} PRIVATE "dummy_tests.m")

set_target_properties(${TESTS_NAME} PROPERTIES
    MACOSX_BUNDLE_INFO_PLIST "Xcode_TestTarget-Info.plist.in"

### IMPORTANT PART ###
# Without the hack below, '/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Frameworks'
# is always added to 'Framework search paths', regardless of the target device - that is, not `iPhoneSimulator.platform` when built for sim.
# This workaround stopped working after CMake version 3.19 (confirmed working with 3.18.5).
# For more info see https://gitlab.kitware.com/cmake/cmake/-/issues/19809

#    XCODE_ATTRIBUTE_FRAMEWORK_SEARCH_PATHS "\$(inherited) "
)
