cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB shc)

include_directories(${CMAKE_BINARY_DIR}
    src
    shc/external
    shc/include
    shc/src)

if(WIN32)
	set (Qt5Gui_DIR "C:\\Qt\\5.10.1\\msvc2013_64\\lib\\cmake\\Qt5Gui\\")
	set (Qt5Widgets_DIR "C:\\Qt\\5.10.1\\msvc2013_64\\lib\\cmake\\Qt5Widgets\\")
endif(WIN32)	

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
configure_file("shc/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  shc/external/miniupnpc/connecthostport.c
  shc/external/miniupnpc/igd_desc_parse.c
  shc/external/miniupnpc/minisoap.c
  shc/external/miniupnpc/miniupnpc.c
  shc/external/miniupnpc/miniwget.c
  shc/external/miniupnpc/minixml.c
  shc/external/miniupnpc/portlistingparse.c
  shc/external/miniupnpc/receivedata.c
  shc/external/miniupnpc/upnpcommands.c
  shc/external/miniupnpc/upnpreplyparse.c
  shc/src/Common/Base58.cpp
  shc/src/Common/CommandLine.cpp
  shc/src/Common/Util.cpp
  shc/src/Common/StringTools.cpp
  shc/src/Common/JsonValue.cpp
  shc/src/Common/ConsoleTools.cpp
  shc/src/Common/MemoryInputStream.cpp
  shc/src/Common/PathTools.cpp
  shc/src/Common/StdInputStream.cpp
  shc/src/Common/StdOutputStream.cpp
  shc/src/Common/StreamTools.cpp
  shc/src/Common/StringOutputStream.cpp
  shc/src/Common/StringView.cpp
  shc/src/Common/VectorOutputStream.cpp
  shc/src/crypto/blake256.c
  shc/src/crypto/chacha8.c
  shc/src/crypto/crypto-ops-data.c
  shc/src/crypto/crypto-ops.c
  shc/src/crypto/crypto.cpp
  shc/src/crypto/groestl.c
  shc/src/crypto/hash-extra-blake.c
  shc/src/crypto/hash-extra-groestl.c
  shc/src/crypto/hash-extra-jh.c
  shc/src/crypto/hash-extra-skein.c
  shc/src/crypto/hash.c
  shc/src/crypto/jh.c
  shc/src/crypto/keccak.c
  shc/src/crypto/oaes_lib.c
  shc/src/crypto/random.c
  shc/src/crypto/skein.c
  shc/src/crypto/slow-hash.c
  shc/src/crypto/slow-hash.cpp
  shc/src/crypto/tree-hash.c
  shc/src/CryptoNoteCore/BlockchainIndices.cpp
  shc/src/CryptoNoteCore/BlockchainMessages.cpp
  shc/src/CryptoNoteCore/BlockIndex.cpp
  shc/src/CryptoNoteCore/CoreConfig.cpp
  shc/src/CryptoNoteCore/CryptoNoteBasic.cpp
  shc/src/CryptoNoteCore/CryptoNoteTools.cpp
  shc/src/CryptoNoteCore/Currency.cpp
  shc/src/CryptoNoteCore/MinerConfig.cpp
  shc/src/CryptoNoteCore/Transaction.cpp
  shc/src/CryptoNoteCore/Account.cpp
  shc/src/CryptoNoteCore/Blockchain.cpp
  shc/src/CryptoNoteCore/Checkpoints.cpp
  shc/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  shc/src/CryptoNoteCore/Core.cpp
  shc/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  shc/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  shc/src/CryptoNoteCore/Difficulty.cpp
  shc/src/CryptoNoteCore/IBlock.cpp
  shc/src/CryptoNoteCore/Miner.cpp
  shc/src/CryptoNoteCore/TransactionExtra.cpp
  shc/src/CryptoNoteCore/TransactionPool.cpp
  shc/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  shc/src/CryptoNoteCore/TransactionUtils.cpp
  shc/src/InProcessNode/InProcessNode.cpp
  shc/src/InProcessNode/InProcessNodeErrors.cpp
  shc/src/NodeRpcProxy/NodeErrors.cpp
  shc/src/NodeRpcProxy/NodeRpcProxy.cpp
  shc/src/P2p/NetNodeConfig.cpp
  shc/src/Serialization/BinaryInputStreamSerializer.cpp
  shc/src/Serialization/BinaryOutputStreamSerializer.cpp
  shc/src/Serialization/JsonInputValueSerializer.cpp
  shc/src/Serialization/JsonOutputStreamSerializer.cpp
  shc/src/Serialization/KVBinaryInputStreamSerializer.cpp
  shc/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  shc/src/Serialization/SerializationOverloads.cpp
  shc/src/Transfers/BlockchainSynchronizer.cpp
  shc/src/Transfers/SynchronizationState.cpp
  shc/src/Transfers/TransfersConsumer.cpp
  shc/src/Transfers/TransfersContainer.cpp
  shc/src/Transfers/TransfersSubscription.cpp
  shc/src/Transfers/TransfersSynchronizer.cpp
  shc/src/Wallet/LegacyKeysImporter.cpp
  shc/src/Wallet/WalletAsyncContextCounter.cpp
  shc/src/Wallet/WalletErrors.cpp
  shc/src/WalletLegacy/KeysStorage.cpp
  shc/src/WalletLegacy/WalletLegacy.cpp
  shc/src/WalletLegacy/WalletHelper.cpp
  shc/src/WalletLegacy/WalletLegacySerializer.cpp
  shc/src/WalletLegacy/WalletLegacySerialization.cpp
  shc/src/WalletLegacy/WalletTransactionSender.cpp
  shc/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  shc/src/WalletLegacy/WalletUserTransactionsCache.cpp
  shc/src/System/ContextGroup.cpp
  shc/src/System/Event.cpp
  shc/src/System/EventLock.cpp
  shc/src/System/InterruptedException.cpp
  shc/src/System/Ipv4Address.cpp
  shc/src/System/TcpStream.cpp
  shc/src/HTTP/HttpRequest.cpp
  shc/src/HTTP/HttpParser.cpp
  shc/src/HTTP/HttpParserErrorCodes.cpp
  shc/src/HTTP/HttpResponse.cpp
  shc/src/Rpc/HttpClient.cpp
  shc/src/Rpc/JsonRpc.cpp
  shc/src/P2p/NetNode.cpp
  shc/src/P2p/LevinProtocol.cpp
  shc/src/P2p/NetNodeConfig.cpp
  shc/src/P2p/PeerListManager.cpp
  shc/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  shc/src/Logging/ILogger.cpp
  shc/src/Logging/LoggerGroup.cpp
  shc/src/Logging/CommonLogger.cpp
  shc/src/Logging/LoggerManager.cpp
  shc/src/Logging/FileLogger.cpp
  shc/src/Logging/StreamLogger.cpp
  shc/src/Logging/ConsoleLogger.cpp
  shc/src/Logging/LoggerMessage.cpp
  shc/src/Logging/LoggerRef.cpp
  shc/src/BlockchainExplorer/BlockchainExplorer.cpp
  shc/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  shc/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
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
  include_directories(shc/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} shc/external/miniupnpc/minissdpc.c)
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

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
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

include_directories(shc/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES shc/src/Platform/${PLATFORM_DIR}/System/*)
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
