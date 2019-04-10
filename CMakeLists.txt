cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB realtradex)

include_directories(${CMAKE_BINARY_DIR}
    src
    realtradex/external
    realtradex/include
    realtradex/src)

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
configure_file("realtradex/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  realtradex/external/miniupnpc/connecthostport.c
  realtradex/external/miniupnpc/igd_desc_parse.c
  realtradex/external/miniupnpc/minisoap.c
  realtradex/external/miniupnpc/miniupnpc.c
  realtradex/external/miniupnpc/miniwget.c
  realtradex/external/miniupnpc/minixml.c
  realtradex/external/miniupnpc/portlistingparse.c
  realtradex/external/miniupnpc/receivedata.c
  realtradex/external/miniupnpc/upnpcommands.c
  realtradex/external/miniupnpc/upnpreplyparse.c
  realtradex/src/Common/Base58.cpp
  realtradex/src/Common/CommandLine.cpp
  realtradex/src/Common/Util.cpp
  realtradex/src/Common/StringTools.cpp
  realtradex/src/Common/JsonValue.cpp
  realtradex/src/Common/ConsoleTools.cpp
  realtradex/src/Common/MemoryInputStream.cpp
  realtradex/src/Common/PathTools.cpp
  realtradex/src/Common/StdInputStream.cpp
  realtradex/src/Common/StdOutputStream.cpp
  realtradex/src/Common/StreamTools.cpp
  realtradex/src/Common/StringOutputStream.cpp
  realtradex/src/Common/StringView.cpp
  realtradex/src/Common/VectorOutputStream.cpp
  realtradex/src/crypto/blake256.c
  realtradex/src/crypto/chacha8.c
  realtradex/src/crypto/crypto-ops-data.c
  realtradex/src/crypto/crypto-ops.c
  realtradex/src/crypto/crypto.cpp
  realtradex/src/crypto/groestl.c
  realtradex/src/crypto/hash-extra-blake.c
  realtradex/src/crypto/hash-extra-groestl.c
  realtradex/src/crypto/hash-extra-jh.c
  realtradex/src/crypto/hash-extra-skein.c
  realtradex/src/crypto/hash.c
  realtradex/src/crypto/jh.c
  realtradex/src/crypto/keccak.c
  realtradex/src/crypto/oaes_lib.c
  realtradex/src/crypto/random.c
  realtradex/src/crypto/skein.c
  realtradex/src/crypto/slow-hash.c
  realtradex/src/crypto/slow-hash.cpp
  realtradex/src/crypto/tree-hash.c
  realtradex/src/CryptoNoteCore/BlockchainIndices.cpp
  realtradex/src/CryptoNoteCore/BlockchainMessages.cpp
  realtradex/src/CryptoNoteCore/BlockIndex.cpp
  realtradex/src/CryptoNoteCore/CoreConfig.cpp
  realtradex/src/CryptoNoteCore/CryptoNoteBasic.cpp
  realtradex/src/CryptoNoteCore/CryptoNoteTools.cpp
  realtradex/src/CryptoNoteCore/Currency.cpp
  realtradex/src/CryptoNoteCore/MinerConfig.cpp
  realtradex/src/CryptoNoteCore/Transaction.cpp
  realtradex/src/CryptoNoteCore/Account.cpp
  realtradex/src/CryptoNoteCore/Blockchain.cpp
  realtradex/src/CryptoNoteCore/Checkpoints.cpp
  realtradex/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  realtradex/src/CryptoNoteCore/Core.cpp
  realtradex/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  realtradex/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  realtradex/src/CryptoNoteCore/Difficulty.cpp
  realtradex/src/CryptoNoteCore/IBlock.cpp
  realtradex/src/CryptoNoteCore/Miner.cpp
  realtradex/src/CryptoNoteCore/TransactionExtra.cpp
  realtradex/src/CryptoNoteCore/TransactionPool.cpp
  realtradex/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  realtradex/src/CryptoNoteCore/TransactionUtils.cpp
  realtradex/src/InProcessNode/InProcessNode.cpp
  realtradex/src/InProcessNode/InProcessNodeErrors.cpp
  realtradex/src/NodeRpcProxy/NodeErrors.cpp
  realtradex/src/NodeRpcProxy/NodeRpcProxy.cpp
  realtradex/src/P2p/NetNodeConfig.cpp
  realtradex/src/Serialization/BinaryInputStreamSerializer.cpp
  realtradex/src/Serialization/BinaryOutputStreamSerializer.cpp
  realtradex/src/Serialization/JsonInputValueSerializer.cpp
  realtradex/src/Serialization/JsonOutputStreamSerializer.cpp
  realtradex/src/Serialization/KVBinaryInputStreamSerializer.cpp
  realtradex/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  realtradex/src/Serialization/SerializationOverloads.cpp
  realtradex/src/Transfers/BlockchainSynchronizer.cpp
  realtradex/src/Transfers/SynchronizationState.cpp
  realtradex/src/Transfers/TransfersConsumer.cpp
  realtradex/src/Transfers/TransfersContainer.cpp
  realtradex/src/Transfers/TransfersSubscription.cpp
  realtradex/src/Transfers/TransfersSynchronizer.cpp
  realtradex/src/Wallet/LegacyKeysImporter.cpp
  realtradex/src/Wallet/WalletAsyncContextCounter.cpp
  realtradex/src/Wallet/WalletErrors.cpp
  realtradex/src/WalletLegacy/KeysStorage.cpp
  realtradex/src/WalletLegacy/WalletLegacy.cpp
  realtradex/src/WalletLegacy/WalletHelper.cpp
  realtradex/src/WalletLegacy/WalletLegacySerializer.cpp
  realtradex/src/WalletLegacy/WalletLegacySerialization.cpp
  realtradex/src/WalletLegacy/WalletTransactionSender.cpp
  realtradex/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  realtradex/src/WalletLegacy/WalletUserTransactionsCache.cpp
  realtradex/src/System/ContextGroup.cpp
  realtradex/src/System/Event.cpp
  realtradex/src/System/EventLock.cpp
  realtradex/src/System/InterruptedException.cpp
  realtradex/src/System/Ipv4Address.cpp
  realtradex/src/System/TcpStream.cpp
  realtradex/src/HTTP/HttpRequest.cpp
  realtradex/src/HTTP/HttpParser.cpp
  realtradex/src/HTTP/HttpParserErrorCodes.cpp
  realtradex/src/HTTP/HttpResponse.cpp
  realtradex/src/Rpc/HttpClient.cpp
  realtradex/src/Rpc/JsonRpc.cpp
  realtradex/src/P2p/NetNode.cpp
  realtradex/src/P2p/LevinProtocol.cpp
  realtradex/src/P2p/NetNodeConfig.cpp
  realtradex/src/P2p/PeerListManager.cpp
  realtradex/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  realtradex/src/Logging/ILogger.cpp
  realtradex/src/Logging/LoggerGroup.cpp
  realtradex/src/Logging/CommonLogger.cpp
  realtradex/src/Logging/LoggerManager.cpp
  realtradex/src/Logging/FileLogger.cpp
  realtradex/src/Logging/StreamLogger.cpp
  realtradex/src/Logging/ConsoleLogger.cpp
  realtradex/src/Logging/LoggerMessage.cpp
  realtradex/src/Logging/LoggerRef.cpp
  realtradex/src/BlockchainExplorer/BlockchainExplorer.cpp
  realtradex/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  realtradex/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
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
  include_directories(realtradex/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/realtradexwallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} realtradex/external/miniupnpc/minissdpc.c)
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

    set(MACOSX_BUNDLE_ICON_FILE realtradex.icns)
    set(APPLICATION_ICON src/images/realtradex.icns)
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

include_directories(realtradex/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES realtradex/src/Platform/${PLATFORM_DIR}/System/*)
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
