set(
  all_src
  ImCurveEdit.cpp
  ImGradient.cpp
  ImGuizmo.cpp
  ImSequencer.cpp
)

set(
  all_headers
  ../include/ImGuizmo/ImGuizmo.h
  ../include/ImGuizmo/ImCurveEdit.h
  ../include/ImGuizmo/ImGradient.h
  ../include/ImGuizmo/ImSequencer.h
)

include_directories(
  ../include
  ../include/ImGuizmo
  ../../
  ../../imgui
)


add_library(ImGuizmo ${all_src} ${all_headers})

set_target_properties(ImGuizmo PROPERTIES
	CXX_STANDARD 20
    CXX_STANDARD_REQUIRED YES
    CXX_EXTENSIONS NO
    VERSION ${PROJECT_VERSION}
    SOVERSION ${PROJECT_VERSION_MAJOR}
)

if(MSVC)
  set_target_properties(ImGuizmo PROPERTIES
    COMPILE_PDB_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}"
    COMPILE_PDB_NAME ImGuizmo
  )

  install(
    FILES "${CMAKE_CURRENT_BINARY_DIR}/$<CONFIG>/ImGuizmo.pdb"
    DESTINATION ${CMAKE_INSTALL_LIBDIR}
    OPTIONAL
  )
endif()

if (BUILD_SHARED_LIBS)
  target_compile_definitions(ImGuizmo
    PUBLIC
      ImGuizmo_SHARED
  )
endif()

source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}" PREFIX "src" FILES ${all_src})
source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}/../include" PREFIX "include" FILES ${all_headers})

install(
  TARGETS ImGuizmo
  EXPORT ImGuizmoConfig
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

install(
  EXPORT ImGuizmoConfig
  NAMESPACE ImGuizmo::
  DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/ImGuizmo"
)

include(CMakePackageConfigHelpers)

write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/ImGuizmoConfigVersion.cmake"
  COMPATIBILITY SameMajorVersion
)

install(
  FILES "${CMAKE_CURRENT_BINARY_DIR}/ImGuizmoConfigVersion.cmake"
  DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/ImGuizmo"
)

