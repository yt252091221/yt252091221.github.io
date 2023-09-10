---
layout: post
title: Darknet_ros在Ubuntu20.04中的编译
header-img: "img/hezuo.png"
tag:
- 仿真
- XTDrone
- UAV
description: "编译中的问题和解决方案"
---


# 按照```CUDA```使能流程安装新版本```OpenCV```

<blockquote>
<a href="https://yt252091221.github.io/2021/10/19/CUDA/" target="_blank">文章链接</a>
</blockquote>

- 建议将其中的```checkout``` 版本改为3.4.14，这样可以直接使用后文文档，若有需要使用硬件相机，在```cmake```中加入```-D WITH_GSTREAMER=ON```

- 下载过慢可以直接到<a href="https://opencv.org/releases/" target="_blank">OpenCV官网</a>下载对应版本，如依此操作，则可以省去```checkout```步骤

- 若不需要```Cuda```版，亦可以单独编译```OpenCV```

# 修改```cv_bridgeConfig.cmake```文件

- 由于卸载原生```OpenCV```的过程中可能会顺带卸载```cv_bridge```

- 查看```/opt/ros/noetic/share```目录下是否有```cv_bridge```文件夹，若没有则重新安装

```cpp
sudo apt-get install ros-noetic-cv-bridge
```

- 将```/opt/ros/noetic/share/cv_bridge/cmake/cv_bridgeConfig.cmake```文件内容替换如下

```cpp
# generated from catkin/cmake/template/pkgConfig.cmake.in

# append elements to a list and remove existing duplicates from the list
# copied from catkin/cmake/list_append_deduplicate.cmake to keep pkgConfig
# self contained
macro(_list_append_deduplicate listname)
  if(NOT "${ARGN}" STREQUAL "")
    if(${listname})
      list(REMOVE_ITEM ${listname} ${ARGN})
    endif()
    list(APPEND ${listname} ${ARGN})
  endif()
endmacro()

# append elements to a list if they are not already in the list
# copied from catkin/cmake/list_append_unique.cmake to keep pkgConfig
# self contained
macro(_list_append_unique listname)
  foreach(_item ${ARGN})
    list(FIND ${listname} ${_item} _index)
    if(_index EQUAL -1)
      list(APPEND ${listname} ${_item})
    endif()
  endforeach()
endmacro()

# pack a list of libraries with optional build configuration keywords
# copied from catkin/cmake/catkin_libraries.cmake to keep pkgConfig
# self contained
macro(_pack_libraries_with_build_configuration VAR)
  set(${VAR} "")
  set(_argn ${ARGN})
  list(LENGTH _argn _count)
  set(_index 0)
  while(${_index} LESS ${_count})
    list(GET _argn ${_index} lib)
    if("${lib}" MATCHES "^(debug|optimized|general)$")
      math(EXPR _index "${_index} + 1")
      if(${_index} EQUAL ${_count})
        message(FATAL_ERROR "_pack_libraries_with_build_configuration() the list of libraries '${ARGN}' ends with '${lib}' which is a build configuration keyword and must be followed by a library")
      endif()
      list(GET _argn ${_index} library)
      list(APPEND ${VAR} "${lib}${CATKIN_BUILD_CONFIGURATION_KEYWORD_SEPARATOR}${library}")
    else()
      list(APPEND ${VAR} "${lib}")
    endif()
    math(EXPR _index "${_index} + 1")
  endwhile()
endmacro()

# unpack a list of libraries with optional build configuration keyword prefixes
# copied from catkin/cmake/catkin_libraries.cmake to keep pkgConfig
# self contained
macro(_unpack_libraries_with_build_configuration VAR)
  set(${VAR} "")
  foreach(lib ${ARGN})
    string(REGEX REPLACE "^(debug|optimized|general)${CATKIN_BUILD_CONFIGURATION_KEYWORD_SEPARATOR}(.+)$" "\\1;\\2" lib "${lib}")
    list(APPEND ${VAR} "${lib}")
  endforeach()
endmacro()


if(cv_bridge_CONFIG_INCLUDED)
  return()
endif()
set(cv_bridge_CONFIG_INCLUDED TRUE)

# set variables for source/devel/install prefixes
if("FALSE" STREQUAL "TRUE")
  set(cv_bridge_SOURCE_PREFIX /tmp/binarydeb/ros-noetic-cv-bridge-1.16.2)
  set(cv_bridge_DEVEL_PREFIX /tmp/binarydeb/ros-noetic-cv-bridge-1.16.2/.obj-x86_64-linux-gnu/devel)
  set(cv_bridge_INSTALL_PREFIX "")
  set(cv_bridge_PREFIX ${cv_bridge_DEVEL_PREFIX})
else()
  set(cv_bridge_SOURCE_PREFIX "")
  set(cv_bridge_DEVEL_PREFIX "")
  set(cv_bridge_INSTALL_PREFIX /opt/ros/noetic)
  set(cv_bridge_PREFIX ${cv_bridge_INSTALL_PREFIX})
endif()

# warn when using a deprecated package
if(NOT "" STREQUAL "")
  set(_msg "WARNING: package 'cv_bridge' is deprecated")
  # append custom deprecation text if available
  if(NOT "" STREQUAL "TRUE")
    set(_msg "${_msg} ()")
  endif()
  message("${_msg}")
endif()

# flag project as catkin-based to distinguish if a find_package()-ed project is a catkin project
set(cv_bridge_FOUND_CATKIN_PROJECT TRUE)

#if(NOT "include;/usr/include/opencv4 " STREQUAL " ")
if(NOT "include;/usr/local/include/opencv;/usr/local/include/opencv2 " STREQUAL " ")
  set(cv_bridge_INCLUDE_DIRS "")
  #set(_include_dirs "include;/usr/include/opencv4")
  set(_include_dirs "include;/usr/local/lib;/usr/local/include/opencv;/usr/local/include/opencv2;/usr/local/include;/usr/include")
  if(NOT "https://github.com/ros-perception/vision_opencv/issues " STREQUAL " ")
    set(_report "Check the issue tracker 'https://github.com/ros-perception/vision_opencv/issues' and consider creating a ticket if the problem has not been reported yet.")
  elseif(NOT "http://www.ros.org/wiki/cv_bridge " STREQUAL " ")
    set(_report "Check the website 'http://www.ros.org/wiki/cv_bridge' for information and consider reporting the problem.")
  else()
    set(_report "Report the problem to the maintainer 'Vincent Rabaud <vincent.rabaud@gmail.com>' and request to fix the problem.")
  endif()
  foreach(idir ${_include_dirs})
    if(IS_ABSOLUTE ${idir} AND IS_DIRECTORY ${idir})
      set(include ${idir})
    elseif("${idir} " STREQUAL "include ")
      get_filename_component(include "${cv_bridge_DIR}/../../../include" ABSOLUTE)
      if(NOT IS_DIRECTORY ${include})
        message(FATAL_ERROR "Project 'cv_bridge' specifies '${idir}' as an include dir, which is not found.  It does not exist in '${include}'.  ${_report}")
      endif()
    else()
      message(FATAL_ERROR "Project 'cv_bridge' specifies '${idir}' as an include dir, which is not found.  It does neither exist as an absolute directory nor in '\${prefix}/${idir}'.  ${_report}")
    endif()
    _list_append_unique(cv_bridge_INCLUDE_DIRS ${include})
  endforeach()
endif()

set(libraries "cv_bridge;/usr/local/lib/libopencv_ml.so;/usr/local/lib/libopencv_dnn.so;/usr/local/lib/libopencv_viz.so;/usr/local/lib/libopencv_core.so;/usr/local/lib/libopencv_flann.so;/usr/local/lib/libopencv_photo.so;/usr/local/lib/libopencv_shape.so;/usr/local/lib/libopencv_video.so;/usr/local/lib/libopencv_ml.so.3.4;/usr/local/lib/libopencv_calib3d.so;/usr/local/lib/libopencv_dnn.so.3.4;/usr/local/lib/libopencv_highgui.so;/usr/local/lib/libopencv_imgproc.so;/usr/local/lib/libopencv_videoio.so;/usr/local/lib/libopencv_viz.so.3.4;/usr/local/lib/libopencv_core.so.3.4;/usr/local/lib/libopencv_superres.so;/usr/local/lib/libopencv_flann.so.3.4;/usr/local/lib/libopencv_imgcodecs.so;/usr/local/lib/libopencv_ml.so.3.4.14;/usr/local/lib/libopencv_objdetect.so;/usr/local/lib/libopencv_photo.so.3.4;/usr/local/lib/libopencv_shape.so.3.4;/usr/local/lib/libopencv_stitching.so;/usr/local/lib/libopencv_video.so.3.4;/usr/local/lib/libopencv_videostab.so;/usr/local/lib/libopencv_dnn.so.3.4.14;/usr/local/lib/libopencv_features2d.so;/usr/local/lib/libopencv_viz.so.3.4.14;/usr/local/lib/libopencv_calib3d.so.3.4;/usr/local/lib/libopencv_core.so.3.4.14;/usr/local/lib/libopencv_highgui.so.3.4;/usr/local/lib/libopencv_imgproc.so.3.4;/usr/local/lib/libopencv_videoio.so.3.4;/usr/local/lib/libopencv_flann.so.3.4.14;/usr/local/lib/libopencv_photo.so.3.4.14;/usr/local/lib/libopencv_shape.so.3.4.14;/usr/local/lib/libopencv_superres.so.3.4;/usr/local/lib/libopencv_video.so.3.4.14;/usr/local/lib/libopencv_imgcodecs.so.3.4;/usr/local/lib/libopencv_objdetect.so.3.4;/usr/local/lib/libopencv_stitching.so.3.4;/usr/local/lib/libopencv_videostab.so.3.4;/usr/local/lib/libopencv_calib3d.so.3.4.14;/usr/local/lib/libopencv_features2d.so.3.4;/usr/local/lib/libopencv_highgui.so.3.4.14;/usr/local/lib/libopencv_imgproc.so.3.4.14;/usr/local/lib/libopencv_videoio.so.3.4.14;/usr/local/lib/libopencv_superres.so.3.4.14;/usr/local/lib/libopencv_imgcodecs.so.3.4.14;/usr/local/lib/libopencv_objdetect.so.3.4.14;/usr/local/lib/libopencv_stitching.so.3.4.14;/usr/local/lib/libopencv_videostab.so.3.4.14;/usr/local/lib/libopencv_features2d.so.3.4.14")
foreach(library ${libraries})
  # keep build configuration keywords, target names and absolute libraries as-is
  if("${library}" MATCHES "^(debug|optimized|general)$")
    list(APPEND cv_bridge_LIBRARIES ${library})
  elseif(${library} MATCHES "^-l")
    list(APPEND cv_bridge_LIBRARIES ${library})
  elseif(${library} MATCHES "^-")
    # This is a linker flag/option (like -pthread)
    # There's no standard variable for these, so create an interface library to hold it
    if(NOT cv_bridge_NUM_DUMMY_TARGETS)
      set(cv_bridge_NUM_DUMMY_TARGETS 0)
    endif()
    # Make sure the target name is unique
    set(interface_target_name "catkin::cv_bridge::wrapped-linker-option${cv_bridge_NUM_DUMMY_TARGETS}")
    while(TARGET "${interface_target_name}")
      math(EXPR cv_bridge_NUM_DUMMY_TARGETS "${cv_bridge_NUM_DUMMY_TARGETS}+1")
      set(interface_target_name "catkin::cv_bridge::wrapped-linker-option${cv_bridge_NUM_DUMMY_TARGETS}")
    endwhile()
    add_library("${interface_target_name}" INTERFACE IMPORTED)
    if("${CMAKE_VERSION}" VERSION_LESS "3.13.0")
      set_property(
        TARGET
        "${interface_target_name}"
        APPEND PROPERTY
        INTERFACE_LINK_LIBRARIES "${library}")
    else()
      target_link_options("${interface_target_name}" INTERFACE "${library}")
    endif()
    list(APPEND cv_bridge_LIBRARIES "${interface_target_name}")
  elseif(TARGET ${library})
    list(APPEND cv_bridge_LIBRARIES ${library})
  elseif(IS_ABSOLUTE ${library})
    list(APPEND cv_bridge_LIBRARIES ${library})
  else()
    set(lib_path "")
    set(lib "${library}-NOTFOUND")
    # since the path where the library is found is returned we have to iterate over the paths manually
    foreach(path /opt/ros/noetic/lib;/opt/ros/noetic/lib)
      find_library(lib ${library}
        PATHS ${path}
        NO_DEFAULT_PATH NO_CMAKE_FIND_ROOT_PATH)
      if(lib)
        set(lib_path ${path})
        break()
      endif()
    endforeach()
    if(lib)
      _list_append_unique(cv_bridge_LIBRARY_DIRS ${lib_path})
      list(APPEND cv_bridge_LIBRARIES ${lib})
    else()
      # as a fall back for non-catkin libraries try to search globally
      find_library(lib ${library})
      if(NOT lib)
        message(FATAL_ERROR "Project '${PROJECT_NAME}' tried to find library '${library}'.  The library is neither a target nor built/installed properly.  Did you compile project 'cv_bridge'?  Did you find_package() it before the subdirectory containing its code is included?")
      endif()
      list(APPEND cv_bridge_LIBRARIES ${lib})
    endif()
  endif()
endforeach()

set(cv_bridge_EXPORTED_TARGETS "")
# create dummy targets for exported code generation targets to make life of users easier
foreach(t ${cv_bridge_EXPORTED_TARGETS})
  if(NOT TARGET ${t})
    add_custom_target(${t})
  endif()
endforeach()

set(depends "rosconsole;sensor_msgs")
foreach(depend ${depends})
  string(REPLACE " " ";" depend_list ${depend})
  # the package name of the dependency must be kept in a unique variable so that it is not overwritten in recursive calls
  list(GET depend_list 0 cv_bridge_dep)
  list(LENGTH depend_list count)
  if(${count} EQUAL 1)
    # simple dependencies must only be find_package()-ed once
    if(NOT ${cv_bridge_dep}_FOUND)
      find_package(${cv_bridge_dep} REQUIRED NO_MODULE)
    endif()
  else()
    # dependencies with components must be find_package()-ed again
    list(REMOVE_AT depend_list 0)
    find_package(${cv_bridge_dep} REQUIRED NO_MODULE ${depend_list})
  endif()
  _list_append_unique(cv_bridge_INCLUDE_DIRS ${${cv_bridge_dep}_INCLUDE_DIRS})

  # merge build configuration keywords with library names to correctly deduplicate
  _pack_libraries_with_build_configuration(cv_bridge_LIBRARIES ${cv_bridge_LIBRARIES})
  _pack_libraries_with_build_configuration(_libraries ${${cv_bridge_dep}_LIBRARIES})
  _list_append_deduplicate(cv_bridge_LIBRARIES ${_libraries})
  # undo build configuration keyword merging after deduplication
  _unpack_libraries_with_build_configuration(cv_bridge_LIBRARIES ${cv_bridge_LIBRARIES})

  _list_append_unique(cv_bridge_LIBRARY_DIRS ${${cv_bridge_dep}_LIBRARY_DIRS})
  _list_append_deduplicate(cv_bridge_EXPORTED_TARGETS ${${cv_bridge_dep}_EXPORTED_TARGETS})
endforeach()

set(pkg_cfg_extras "cv_bridge-extras.cmake")
foreach(extra ${pkg_cfg_extras})
  if(NOT IS_ABSOLUTE ${extra})
    set(extra ${cv_bridge_DIR}/${extra})
  endif()
  include(${extra})
endforeach()
```

# 编译

- 删除工作空间下除了```src```外的其余文件，包括```src```文件夹中可能包含旧的```CMakeLists```文件，最好也删除

```cpp
cd ~/catkin_ws

catkin_make -DCMAKE_BUILD_TYPE=Release  DCMAKE_CXX_FLAGS=-DCV__ENABLE_C_API_CTORS
```
