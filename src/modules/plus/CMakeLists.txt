add_library(mltplus MODULE
  subtitles/subtitles.cpp
  consumer_blipflash.c
  factory.c
  filter_affine.c
  filter_charcoal.c
  filter_chroma_hold.c
  filter_chroma.c
  filter_dynamictext.c
  filter_dynamic_loudness.c
  filter_gradientmap.cpp
  filter_hslprimaries.c
  filter_hslrange.c
  filter_invert.c
  filter_lift_gamma_gain.c
  filter_loudness.c
  filter_loudness_meter.c
  filter_lumakey.c
  filter_rgblut.c
  filter_sepia.c
  filter_shape.c
  filter_spot_remover.c
  filter_strobe.c
  filter_subtitle_feed.cpp
  filter_subtitle.cpp
  filter_text.c
  filter_threshold.c
  filter_timer.c
  interp.h
  producer_blipflash.c
  producer_count.c
  producer_pgm.c
  producer_subtitle.c
  transition_affine.c
)

file(GLOB YML "*.yml")
add_custom_target(Other_plus_Files SOURCES
  ${YML}
)

target_compile_options(mltplus PRIVATE ${MLT_COMPILE_OPTIONS})

target_link_libraries(mltplus PRIVATE mlt Threads::Threads)
if(NOT MSVC)
  target_link_libraries(mltplus PRIVATE m)
endif()

if(TARGET PkgConfig::FFTW)
  target_sources(mltplus PRIVATE filter_dance.c filter_fft.c)
  target_link_libraries(mltplus PRIVATE PkgConfig::FFTW)
  target_compile_definitions(mltplus PRIVATE USE_FFTW)
  install(FILES filter_dance.yml filter_fft.yml DESTINATION ${MLT_INSTALL_DATA_DIR}/plus)
endif()

if(TARGET PkgConfig::libebur128)
  target_link_libraries(mltplus PRIVATE PkgConfig::libebur128)
else()
  target_sources(mltplus PRIVATE ebur128/ebur128.c)
  target_include_directories(mltplus PRIVATE ebur128 ebur128/queue) 
  target_compile_definitions(mltplus PRIVATE USE_INTERNAL_LIBEBUR128)
endif()

set_target_properties(mltplus PROPERTIES LIBRARY_OUTPUT_DIRECTORY "${MLT_MODULE_OUTPUT_DIRECTORY}")

install(TARGETS mltplus LIBRARY DESTINATION ${MLT_INSTALL_MODULE_DIR})

install(FILES
  consumer_blipflash.yml
  filter_affine.yml
  filter_charcoal.yml
  filter_chroma_hold.yml
  filter_chroma.yml
  filter_dynamic_loudness.yml
  filter_dynamictext.yml
  filter_gradientmap.yml
  filter_hslprimaries.yml
  filter_hslrange.yml
  filter_invert.yml
  filter_lift_gamma_gain.yml
  filter_loudness_meter.yml
  filter_loudness.yml
  filter_lumakey.yml
  filter_rgblut.yml
  filter_sepia.yml
  filter_shape.yml
  filter_spot_remover.yml
  filter_strobe.yml
  filter_subtitle_feed.yml
  filter_subtitle.yml
  filter_text.yml
  filter_threshold.yml
  filter_timer.yml
  producer_blipflash.yml
  producer_count.yml
  producer_pgm.yml
  transition_affine.yml
  DESTINATION ${MLT_INSTALL_DATA_DIR}/plus
)
