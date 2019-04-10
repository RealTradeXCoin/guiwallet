cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB RTXC)

include_directories(${CMAKE_BINARY_DIR}
    src
    RTXC/external
    RTXC/include
    RTXC/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("RTXC/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  RTXC/external/miniupnpc/connecthostport.c
  RTXC/external/miniupnpc/igd_desc_parse.c
  RTXC/external/miniupnpc/minisoap.c
  RTXC/external/miniupnpc/miniupnpc.c
  RTXC/external/miniupnpc/miniwget.c
  RTXC/external/miniupnpc/minixml.c
  RTXC/external/miniupnpc/portlistingparse.c
  RTXC/external/miniupnpc/receivedata.c
  RTXC/external/miniupnpc/upnpcommands.c
  RTXC/external/miniupnpc/upnpreplyparse.c
  RTXC/src/Common/Base58.cpp
  RTXC/src/Common/CommandLine.cpp
  RTXC/src/Common/Util.cpp
  RTXC/src/Common/StringTools.cpp
  RTXC/src/Common/JsonValue.cpp
  RTXC/src/Common/ConsoleTools.cpp
  RTXC/src/Common/MemoryInputStream.cpp
  RTXC/src/Common/PathTools.cpp
  RTXC/src/Common/StdInputStream.cpp
  RTXC/src/Common/StdOutputStream.cpp
  RTXC/src/Common/StreamTools.cpp
  RTXC/src/Common/StringOutputStream.cpp
  RTXC/src/Common/StringView.cpp
  RTXC/src/Common/VectorOutputStream.cpp
  RTXC/src/crypto/blake256.c
  RTXC/src/crypto/chacha8.c
  RTXC/src/crypto/crypto-ops-data.c
  RTXC/src/crypto/crypto-ops.c
  RTXC/src/crypto/crypto.cpp
  RTXC/src/crypto/groestl.c
  RTXC/src/crypto/hash-extra-blake.c
  RTXC/src/crypto/hash-extra-groestl.c
  RTXC/src/crypto/hash-extra-jh.c
  RTXC/src/crypto/hash-extra-skein.c
  RTXC/src/crypto/hash.c
  RTXC/src/crypto/jh.c
  RTXC/src/crypto/keccak.c
  RTXC/src/crypto/oaes_lib.c
  RTXC/src/crypto/random.c
  RTXC/src/crypto/skein.c
  RTXC/src/crypto/slow-hash.c
  RTXC/src/crypto/slow-hash.cpp
  RTXC/src/crypto/tree-hash.c
  RTXC/src/CryptoNoteCore/BlockchainIndices.cpp
  RTXC/src/CryptoNoteCore/BlockchainMessages.cpp
  RTXC/src/CryptoNoteCore/BlockIndex.cpp
  RTXC/src/CryptoNoteCore/CoreConfig.cpp
  RTXC/src/CryptoNoteCore/CryptoNoteBasic.cpp
  RTXC/src/CryptoNoteCore/CryptoNoteTools.cpp
  RTXC/src/CryptoNoteCore/Currency.cpp
  RTXC/src/CryptoNoteCore/MinerConfig.cpp
  RTXC/src/CryptoNoteCore/Transaction.cpp
  RTXC/src/CryptoNoteCore/Account.cpp
  RTXC/src/CryptoNoteCore/Blockchain.cpp
  RTXC/src/CryptoNoteCore/Checkpoints.cpp
  RTXC/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  RTXC/src/CryptoNoteCore/Core.cpp
  RTXC/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  RTXC/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  RTXC/src/CryptoNoteCore/Difficulty.cpp
  RTXC/src/CryptoNoteCore/IBlock.cpp
  RTXC/src/CryptoNoteCore/Miner.cpp
  RTXC/src/CryptoNoteCore/TransactionExtra.cpp
  RTXC/src/CryptoNoteCore/TransactionPool.cpp
  RTXC/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  RTXC/src/CryptoNoteCore/TransactionUtils.cpp
  RTXC/src/InProcessNode/InProcessNode.cpp
  RTXC/src/InProcessNode/InProcessNodeErrors.cpp
  RTXC/src/NodeRpcProxy/NodeErrors.cpp
  RTXC/src/NodeRpcProxy/NodeRpcProxy.cpp
  RTXC/src/P2p/NetNodeConfig.cpp
  RTXC/src/Serialization/BinaryInputStreamSerializer.cpp
  RTXC/src/Serialization/BinaryOutputStreamSerializer.cpp
  RTXC/src/Serialization/JsonInputValueSerializer.cpp
  RTXC/src/Serialization/JsonOutputStreamSerializer.cpp
  RTXC/src/Serialization/KVBinaryInputStreamSerializer.cpp
  RTXC/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  RTXC/src/Serialization/SerializationOverloads.cpp
  RTXC/src/Transfers/BlockchainSynchronizer.cpp
  RTXC/src/Transfers/SynchronizationState.cpp
  RTXC/src/Transfers/TransfersConsumer.cpp
  RTXC/src/Transfers/TransfersContainer.cpp
  RTXC/src/Transfers/TransfersSubscription.cpp
  RTXC/src/Transfers/TransfersSynchronizer.cpp
  RTXC/src/Wallet/LegacyKeysImporter.cpp
  RTXC/src/Wallet/WalletAsyncContextCounter.cpp
  RTXC/src/Wallet/WalletErrors.cpp
  RTXC/src/WalletLegacy/KeysStorage.cpp
  RTXC/src/WalletLegacy/WalletLegacy.cpp
  RTXC/src/WalletLegacy/WalletHelper.cpp
  RTXC/src/WalletLegacy/WalletLegacySerializer.cpp
  RTXC/src/WalletLegacy/WalletLegacySerialization.cpp
  RTXC/src/WalletLegacy/WalletTransactionSender.cpp
  RTXC/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  RTXC/src/WalletLegacy/WalletUserTransactionsCache.cpp
  RTXC/src/System/ContextGroup.cpp
  RTXC/src/System/Event.cpp
  RTXC/src/System/EventLock.cpp
  RTXC/src/System/InterruptedException.cpp
  RTXC/src/System/Ipv4Address.cpp
  RTXC/src/System/TcpStream.cpp
  RTXC/src/HTTP/HttpRequest.cpp
  RTXC/src/HTTP/HttpParser.cpp
  RTXC/src/HTTP/HttpParserErrorCodes.cpp
  RTXC/src/HTTP/HttpResponse.cpp
  RTXC/src/Rpc/HttpClient.cpp
  RTXC/src/Rpc/JsonRpc.cpp
  RTXC/src/P2p/NetNode.cpp
  RTXC/src/P2p/LevinProtocol.cpp
  RTXC/src/P2p/NetNodeConfig.cpp
  RTXC/src/P2p/PeerListManager.cpp
  RTXC/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  RTXC/src/Logging/ILogger.cpp
  RTXC/src/Logging/LoggerGroup.cpp
  RTXC/src/Logging/CommonLogger.cpp
  RTXC/src/Logging/LoggerManager.cpp
  RTXC/src/Logging/FileLogger.cpp
  RTXC/src/Logging/StreamLogger.cpp
  RTXC/src/Logging/ConsoleLogger.cpp
  RTXC/src/Logging/LoggerMessage.cpp
  RTXC/src/Logging/LoggerRef.cpp
  RTXC/src/BlockchainExplorer/BlockchainExplorer.cpp
  RTXC/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  RTXC/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(RTXC/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/RTXCwallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} RTXC/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE RTXC.icns)
    set(APPLICATION_ICON src/images/RTXC.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(RTXC/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES RTXC/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
