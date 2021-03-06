add_library(mltsdl MODULE
  consumer_sdl_audio.c
  consumer_sdl_preview.c
  consumer_sdl_still.c
  consumer_sdl.c
  factory.c
)

target_link_libraries(mltsdl mlt m Threads::Threads PkgConfig::sdl)

if(TARGET PkgConfig::sdl_image)
  target_sources(mltsdl PRIVATE producer_sdl_image.c)
  target_link_libraries(mltsdl PkgConfig::sdl_image)
  target_compile_definitions(mltsdl PRIVATE WITH_SDL_IMAGE)
  install(FILES producer_sdl_image.yml DESTINATION ${CMAKE_INSTALL_DATADIR}/mlt/sdl)
endif()

# Create module in parent directory, for the benefit of "source setenv".
set_target_properties(mltsdl PROPERTIES LIBRARY_OUTPUT_DIRECTORY ..)

install(TARGETS mltsdl LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/mlt)

install(FILES
  consumer_sdl_audio.yml
  consumer_sdl_preview.yml
  consumer_sdl_still.yml
  consumer_sdl.yml
  DESTINATION ${CMAKE_INSTALL_DATADIR}/mlt/sdl
)
