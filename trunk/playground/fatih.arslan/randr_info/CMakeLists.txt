
if( XRANDR_1_2_FOUND )
    macro_optional_add_subdirectory(module)
endif( XRANDR_1_2_FOUND )

configure_file (config-randr.h.cmake
                ${CMAKE_CURRENT_BINARY_DIR}/config-randr.h )

include_directories( ${X11_Xrandr_INCLUDE_PATH}  )

########### next target ###############


set(randrinternal_PART_SRCS 
   ktimerdialog.cpp
   randr.cpp 
   randrdisplay.cpp
   collapsiblewidget.cpp
   legacyrandrscreen.cpp) 

if( XRANDR_1_2_FOUND )
   set(randrinternal_PART_SRCS ${randrinternal_PART_SRCS} layoutmanager.cpp 
       outputconfig.cpp outputgraphicsitem.cpp randrconfig.cpp randrcrtc.cpp 
       randrmode.cpp randroutput.cpp randrscreen.cpp)
endif( XRANDR_1_2_FOUND )

set(kcm_randr_PART_SRCS 
   krandrmodule.cpp 
   glinfo.cpp
   legacyrandrconfig.cpp
   ${randrinternal_PART_SRCS})

kde4_add_ui_files(kcm_randr_PART_SRCS
   legacyrandrconfigbase.ui
   randrconfigbase.ui
   outputconfigbase.ui)


kde4_add_plugin(kcm_randr ${kcm_randr_PART_SRCS})


target_link_libraries(kcm_randr  ${KDE4_KDEUI_LIBS} ${X11_Xrandr_LIB}
    ${X11_LIBRARIES} ${QT_QTGUI_LIBRARY} ${X11_X11_LIB} ${OPENGL_gl_LIBRARY} ${KDE4_KIO_LIBS})



install(TARGETS kcm_randr  DESTINATION ${PLUGIN_INSTALL_DIR} )


########### next target ###############

set(krandrtray_SRCS 
   main.cpp 
   krandrtray.cpp 
   krandrapp.cpp 
   krandrpassivepopup.cpp ${randrinternal_PART_SRCS})


kde4_add_executable(krandrtray ${krandrtray_SRCS})

target_link_libraries(krandrtray ${KDE4_KCMUTILS_LIBS} ${X11_Xrandr_LIB} ${X11_LIBRARIES})

install(TARGETS krandrtray ${INSTALL_TARGETS_DEFAULT_ARGS})


########### next target ###############

install(PROGRAMS krandrstartup DESTINATION ${BIN_INSTALL_DIR})


########### install files ###############

install( FILES krandrtray.desktop  DESTINATION  ${XDG_APPS_INSTALL_DIR} )
install( FILES randr.desktop  DESTINATION  ${SERVICES_INSTALL_DIR} )
