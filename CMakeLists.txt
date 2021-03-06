cmake_minimum_required(VERSION 2.8)

if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
  if(DEFINED ENV{VITASDK})
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VITASDK}/share/vita.toolchain.cmake" CACHE PATH "toolchain file")
  else()
    message(FATAL_ERROR "Please define VITASDK to point to your SDK path!")
  endif()
endif()

project(launcher)
include("${VITASDK}/share/vita.cmake" REQUIRED)

set(VITA_APP_NAME "Launcher")
set(VITA_TITLEID  "SMLA00001")
set(VITA_VERSION  "02.70")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu11")
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wno-sign-compare -Wno-unused-parameter -Wno-psabi -Wunused-variable -Wwrite-strings -fpermissive -std=c++17 -Wimplicit-fallthrough")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -fpermissive")

add_definitions(-DSQLITE_OS_OTHER=1 -DSQLITE_TEMP_STORE=3 -DSQLITE_THREADSAFE=0)

include_directories(
  src
  sqlite-3.6.23.1
)

add_executable(${PROJECT_NAME}
  src/launcher.cpp
  src/gui.cpp
  src/textures.cpp
  src/fs.cpp
  src/sfo.cpp
  src/game.cpp
  src/main.cpp
  src/inifile.c
  src/vita_sqlite.c
  src/db.cpp
  src/config.cpp
  src/eboot.cpp
  src/iso.cpp
  src/cso.cpp
  src/style.cpp
  src/ime_dialog.cpp
  src/net.cpp
  sqlite-3.6.23.1/sqlite3.c
)

target_link_libraries(${PROJECT_NAME}
  vita2d
  png
  imgui_vita2d
  stdc++
  c
  z
  SceCommonDialog_stub
  SceLibKernel_stub
  SceDisplay_stub
  SceGxm_stub
  SceSysmodule_stub
  SceCtrl_stub
  SceTouch_stub
  SceAudio_stub
  SceHid_stub
  SceNet_stub
  SceNetCtl_stub
  SceHttp_stub
  SceSsl_stub
  ScePspnetAdhoc_stub
  ScePromoterUtil_stub
  SceShellSvc_stub
  m
  SceAppMgr_stub
  SceAppUtil_stub
  SceShaccCg_stub
)

vita_create_self(eboot.bin ${PROJECT_NAME} UNSAFE)
vita_create_vpk(${PROJECT_NAME}.vpk ${VITA_TITLEID} eboot.bin
  VERSION ${VITA_VERSION}
  NAME ${VITA_APP_NAME}
  FILE sce_sys/icon0.png sce_sys/icon0.png
  FILE sce_sys/livearea/contents/bg.png sce_sys/livearea/contents/bg.png
  FILE sce_sys/livearea/contents/startup.png sce_sys/livearea/contents/startup.png
  FILE sce_sys/livearea/contents/template.xml sce_sys/livearea/contents/template.xml
  FILE thumbnails_db/a2600.db thumbnails/a2600.db
  FILE thumbnails_db/a5200.db thumbnails/a5200.db
  FILE thumbnails_db/a7800.db thumbnails/a7800.db
  FILE thumbnails_db/aLynx.db thumbnails/aLynx.db
  FILE thumbnails_db/bandai.db thumbnails/bandai.db
  FILE thumbnails_db/c64.db thumbnails/c64.db
  FILE thumbnails_db/gaw.db thumbnails/gaw.db
  FILE thumbnails_db/gb.db thumbnails/gb.db
  FILE thumbnails_db/gba.db thumbnails/gba.db
  FILE thumbnails_db/gbc.db thumbnails/gbc.db
  FILE thumbnails_db/ggear.db thumbnails/ggear.db
  FILE thumbnails_db/mame2k.db thumbnails/mame2k.db
  FILE thumbnails_db/mame2k3.db thumbnails/mame2k3.db
  FILE thumbnails_db/mdrive.db thumbnails/mdrive.db
  FILE thumbnails_db/msx2.db thumbnails/msx2.db
  FILE thumbnails_db/msystem.db thumbnails/msystem.db
  FILE thumbnails_db/n64.db thumbnails/n64.db
  FILE thumbnails_db/nec.db thumbnails/nec.db
  FILE thumbnails_db/neogeo.db thumbnails/neogeo.db
  FILE thumbnails_db/neogeopc.db thumbnails/neogeopc.db
  FILE thumbnails_db/nes.db thumbnails/nes.db
  FILE thumbnails_db/ps1.db thumbnails/ps1.db
  FILE thumbnails_db/saturn.db thumbnails/saturn.db
  FILE thumbnails_db/snes.db thumbnails/snes.db
  FILE thumbnails_db/tgrafx.db thumbnails/tgrafx.db
  FILE thumbnails_db/vectrex.db thumbnails/vectrex.db
  FILE thumbnails_db/amiga.db thumbnails/amiga.db
  FILE thumbnails_db/sega32x.db thumbnails/sega32x.db
  FILE thumbnails_db/segacd.db thumbnails/segacd.db
  FILE thumbnails_db/msx1.db thumbnails/msx1.db
  FILE thumbnails_db/neogeocd.db thumbnails/neogeocd.db
  FILE thumbnails_db/scummvm.db thumbnails/scummvm.db
  FILE thumbnails_db/dreamcast.db thumbnails/dreamcast.db
  FILE thumbnails_db/mame_mapping.db thumbnails/mame_mapping.db
  FILE noicon.png noicon.png
  FILE favorite.png favorite.png
  FILE square.png square.png
  FILE triangle.png triangle.png
  FILE circle.png circle.png
  FILE cross.png cross.png
  FILE start.png start.png
  FILE folder.png folder.png
  FILE selected.png selected.png
  FILE default_style.ini default_style.ini
)
