cmake_minimum_required(VERSION 2.6)

PROJECT(vtkPointSetSurfaceReconstruction)
ENABLE_TESTING()

FIND_PACKAGE(VTK REQUIRED)
INCLUDE(${VTK_USE_FILE})

SET(BUILD_PARAVIEW_PLUGIN OFF CACHE BOOL "Build Paraview plugin?")

if(BUILD_PARAVIEW_PLUGIN)
  
  FIND_PACKAGE(ParaView REQUIRED)
  INCLUDE(${PARAVIEW_USE_FILE})
  
  INCLUDE_DIRECTORIES(${INCLUDE_DIRECTORIES} ../)
  
  ADD_PARAVIEW_PLUGIN(PointSetSurfaceReconstruction "1.0"
    SERVER_MANAGER_XML ../vtkPointSetSurfaceReconstruction.xml
  SERVER_MANAGER_SOURCES ../vtkPointSetSurfaceReconstruction.cxx ../vtkVoxelizePolyData.cxx)

endif(BUILD_PARAVIEW_PLUGIN)

################
ADD_EXECUTABLE(vtkPointSetSurfaceReconstructionExample vtkPointSetSurfaceReconstructionExample.cxx
vtkPointSetSurfaceReconstruction.cxx 
vtkVoxelizePolyData.cxx)
TARGET_LINK_LIBRARIES(vtkPointSetSurfaceReconstructionExample vtkHybrid)

#################
ADD_EXECUTABLE(vtkPointSetSurfaceReconstructionTest vtkPointSetSurfaceReconstructionTest.cxx 
vtkPointSetSurfaceReconstruction.cxx
vtkVoxelizePolyData.cxx
)
TARGET_LINK_LIBRARIES(vtkPointSetSurfaceReconstructionTest vtkHybrid vtkInfovis)
#ADD_TEST(vtkPointSetSurfaceReconstructionTest vtkPointSetSurfaceReconstructionTest ${CMAKE_SOURCE_DIR}/data/Input.vtp output.vtp)
ADD_TEST(vtkPointSetSurfaceReconstructionTest vtkPointSetSurfaceReconstructionTest Input.vtp Output.vtp)