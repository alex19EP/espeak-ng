add_library(speechPlayer
  src/frame.cpp
  src/speechPlayer.cpp
  src/speechWaveGenerator.cpp
)
target_include_directories(speechPlayer PUBLIC include)
