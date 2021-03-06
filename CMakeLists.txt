#  This file is part of Nifti2Dicom, is an open source converter from 
#  3D NIfTI images to 2D DICOM series.
#
#  Copyright (C) 2008, 2009, 2010 Daniele E. Domenichelli <daniele.domenichelli@gmail.com>
#
#  Nifti2Dicom is free software: you can redistribute it and/or modify
#  it under the terms of the GNU General Public License as published by
#  the Free Software Foundation, either version 3 of the License, or
#  (at your option) any later version.
#
#  Nifti2Dicom is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU General Public License for more details.
#
#  You should have received a copy of the GNU General Public License
#  along with Nifti2Dicom.  If not, see <http://www.gnu.org/licenses/>.


cmake_minimum_required(VERSION 2.6.4)
project(Nifti2Dicom)

set(Nifti2Dicom_MAJOR_VERSION "0")
set(Nifti2Dicom_MINOR_VERSION "4")
set(Nifti2Dicom_PATCH_VERSION "7")
set(Nifti2Dicom_VERSION "0.4.7")
set(Nifti2Dicom_INTERNAL_VERSION "${Nifti2Dicom_MAJOR_VERSION}.${Nifti2Dicom_MINOR_VERSION}.${Nifti2Dicom_PATCH_VERSION}")


#Check pkg-config
find_package(PkgConfig)
#TODO error if not found

#Check TCLAP Library [Templatized C++ Command Line Parser] (1.2.0)
pkg_check_modules(TCLAP REQUIRED tclap>=1.2.0)
if(TCLAP_FOUND)
    include_directories(${TCLAP_INCLUDE_DIRS})
else(TCLAP_FOUND)
    message(FATAL_ERROR "TCLAP not found")
endif(TCLAP_FOUND)


# Check ITK Library [Insight ToolKit] (3.17.0)
find_package(ITK)
if(ITK_FOUND)
    include(${ITK_USE_FILE})
else(ITK_FOUND)
    message(FATAL_ERROR "ITK not found")
endif(ITK_FOUND)


#TODO Check GDCM Library [Grassroots DiCoM] (2.1.13)
#find_package(GDCM REQUIRED)
#message("GDCM_VERSION = " ${GDCM_VERSION})
#if(GDCM_FOUND)
#    include(${GDCM_USE_FILE})
#else(GDCM_FOUND)
#    message(FATAL_ERROR "GDCM 2 not found")
#endif(GDCM_FOUND)


#Check Qt4 Library [Qt4] (4.4.0)
find_package(Qt4 REQUIRED)
if(QT_FOUND)
	include(${QT_USE_FILE})
else(QT_FOUND)
    message(FATAL ERROR "Qt4 not found")
endif(QT_FOUND)

#Check VTK Library [Visualization ToolKit]
find_package(VTK REQUIRED)
if(VTK_FOUND)
    include(${VTK_USE_FILE})
else(VTK_FOUND)
    message(FATAL ERROR "VTK not found")
endif(VTK_FOUND)

#TODO Check QVTK

configure_file(${CMAKE_SOURCE_DIR}/Nifti2DicomConfig.h.cmake ${CMAKE_BINARY_DIR}/Nifti2DicomConfig.h)

include_directories(${CMAKE_BINARY_DIR})

#TODO add DebugFull
if(CMAKE_BUILD_TYPE STREQUAL "Debug" OR CMAKE_BUILD_TYPE STREQUAL "RelWithDebugInfo")
  message(STATUS "DEBUG ENABLED")
  add_definitions("-DDEBUG")
endif() #TODO this endif?


add_subdirectory(src)
add_subdirectory(doc)
add_subdirectory(data)
