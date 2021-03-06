cmake_minimum_required (VERSION 2.8.11)
PROJECT (HDF5_TOOLS_H5DIFF)

#-----------------------------------------------------------------------------
# Setup include Directories
#-----------------------------------------------------------------------------
INCLUDE_DIRECTORIES (${HDF5_TOOLS_SRC_DIR}/lib)

# --------------------------------------------------------------------
# Add the h5diff executables
# --------------------------------------------------------------------
add_executable (h5diff
    ${HDF5_TOOLS_H5DIFF_SOURCE_DIR}/h5diff_common.c
    ${HDF5_TOOLS_H5DIFF_SOURCE_DIR}/h5diff_main.c
)
TARGET_NAMING (h5diff ${LIB_TYPE})
TARGET_C_PROPERTIES (h5diff " " " ")
target_link_libraries (h5diff  ${HDF5_TOOLS_LIB_TARGET} ${HDF5_LIB_TARGET})
set_target_properties (h5diff PROPERTIES FOLDER tools)

set (H5_DEP_EXECUTABLES h5diff)

if (H5_HAVE_PARALLEL)
  add_executable (ph5diff
      ${HDF5_TOOLS_H5DIFF_SOURCE_DIR}/h5diff_common.c
      ${HDF5_TOOLS_H5DIFF_SOURCE_DIR}/ph5diff_main.c
  )
  TARGET_NAMING (ph5diff ${LIB_TYPE})
  TARGET_C_PROPERTIES (ph5diff " " " ")
  target_link_libraries (ph5diff  ${HDF5_TOOLS_LIB_TARGET} ${HDF5_LIB_TARGET})
  set_target_properties (ph5diff PROPERTIES FOLDER tools)
endif (H5_HAVE_PARALLEL)

if (BUILD_TESTING)
  # --------------------------------------------------------------------
  # Add the h5diff and test executables
  # --------------------------------------------------------------------
  if (HDF5_BUILD_GENERATORS AND NOT BUILD_SHARED_LIBS)
    add_executable (h5diffgentest ${HDF5_TOOLS_H5DIFF_SOURCE_DIR}/h5diffgentest.c)
    TARGET_NAMING (h5diffgentest ${LIB_TYPE})
    TARGET_C_PROPERTIES (h5diffgentest " " " ")
    target_link_libraries (h5diffgentest ${HDF5_LIB_TARGET})
    set_target_properties (h5diffgentest PROPERTIES FOLDER generator/tools)
    
    #add_test (NAME h5diffgentest COMMAND $<TARGET_FILE:h5diffgentest>)
  endif (HDF5_BUILD_GENERATORS AND NOT BUILD_SHARED_LIBS)

  include (CMakeTests.cmake)
 
endif (BUILD_TESTING)

##############################################################################
##############################################################################
###           I N S T A L L A T I O N                                      ###
##############################################################################
##############################################################################

#-----------------------------------------------------------------------------
# Rules for Installation of tools using make Install target
#-----------------------------------------------------------------------------

#INSTALL_PROGRAM_PDB (h5diff ${HDF5_INSTALL_BIN_DIR} toolsapplications)

install (
    TARGETS
        h5diff
    RUNTIME DESTINATION
        ${HDF5_INSTALL_BIN_DIR}
    COMPONENT
        toolsapplications
)

if (H5_HAVE_PARALLEL)

  #INSTALL_PROGRAM_PDB (ph5diff ${HDF5_INSTALL_BIN_DIR} toolsapplications)

  install (
      TARGETS
          ph5diff
      RUNTIME DESTINATION
          ${HDF5_INSTALL_BIN_DIR}
      COMPONENT
          toolsapplications
  )
endif (H5_HAVE_PARALLEL)
