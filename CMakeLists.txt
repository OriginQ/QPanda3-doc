# Whether or not to enable generating documents.
if(NOT BUILD_DOCS)  
    return()
endif()
message(STATUS "BUILD_DOCS = " ${BUILD_DOCS})


# Dependencies scheme (* - optional):
#
# doxygen_cpp ---------> doxygen -> opencv_docs
#    \                     /        
#     -> doxygen_python* ->        


# Find Doxygen Tool
find_package(Doxygen)
# If the search is successful, proceed to the subsequent document generation step
if(DOXYGEN_FOUND)
    message(STATUS "Doxygen found: ${DOXYGEN_EXECUTABLE}")

    # Doxygen version checking
    if (DOXYGEN_VERSION VERSION_LESS 1.9.8)
    message(WARNING "Found doxygen ${DOXYGEN_VERSION}, version 1.9.8 is used for testing, there is "
                    "a chance your documentation will look different or have some limitations.")
    endif()

    # Create a custom build target for Doxygen 
    # This target does not directly build a file itself, but is used to organize and control the process of generating Doxygen documents.
    add_custom_target(doxygen_docs)

    set(doxyfile "${CMAKE_CURRENT_BINARY_DIR}/Doxyfile")

    set(QPANDA_DOCS_DOXYFILE_IN "Doxyfile.in" CACHE PATH "Doxygen configuration template file (Doxyfile.in)")

    configure_file("${QPANDA_DOCS_DOXYFILE_IN}" ${doxyfile} @ONLY)

    # Create Doxygen's custom build target for generating documentation.
    # DEPENDS specifies the files and resources on which the task depends.
    add_custom_target(
        doxygen_cpp_docs
        COMMAND ${DOXYGEN_EXECUTABLE} ${doxyfile}
        DEPENDS ${doxyfile} ${rootfile}
        COMMENT "Generate documentation with Doxygen"
    )

    # Setting up target dependencies for document generation. 
    # Doxygen depends on doxygen_cpp, which ensures that document generation tasks are executed sequentially.
    add_dependencies(doxygen_docs doxygen_cpp_docs)

else()
    message(WARNING "Doxygen not found!")
endif()


# Setting the Mathjax source folder path.
set(SOURCE_FOLDER "${CMAKE_CURRENT_SOURCE_DIR}/MathJax")
# Set the destination folder path (here it's a new folder in the build directory, adjust as needed)
set(DESTINATION_FOLDER "${CMAKE_CURRENT_BINARY_DIR}/doc/qpanda-3")
# Create the destination folder (if it doesn't exist)
file(MAKE_DIRECTORY ${DESTINATION_FOLDER})
# Copy Mathjax file operation.
file(COPY ${SOURCE_FOLDER} DESTINATION ${DESTINATION_FOLDER} FILES_MATCHING PATTERN "*")