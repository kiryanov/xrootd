
include( XRootDCommon )

if( BUILD_LIBEVENT )
  include_directories( ${LIBEVENT_INCLUDE_DIR} )
  set( LIBEVENT_POLLER_FILES
       XrdClPollerLibEvent.cc
       XrdClPollerLibEvent.hh )
else()
  set( LIBEVENT_POLLER_FILES "" )
endif()

#-------------------------------------------------------------------------------
# Shared library version
#-------------------------------------------------------------------------------
set( XRD_CL_VERSION   1.0.1 )
set( XRD_CL_SOVERSION 1 )

#-------------------------------------------------------------------------------
# The XrdCl lib
#-------------------------------------------------------------------------------
add_library(
  XrdCl
  SHARED
  XrdClLog.cc                 XrdClLog.hh
  XrdClUtils.cc               XrdClUtils.hh
                              XrdClOptimizers.hh
                              XrdClConstants.hh
  XrdClEnv.cc                 XrdClEnv.hh
  XrdClDefaultEnv.cc          XrdClDefaultEnv.hh
  XrdClURL.cc                 XrdClURL.hh
  XrdClStatus.cc              XrdClStatus.hh
  XrdClSocket.cc              XrdClSocket.hh
                              XrdClPoller.hh
  XrdClPollerFactory.cc       XrdClPollerFactory.hh
  XrdClPollerBuiltIn.cc       XrdClPollerBuiltIn.hh
  XrdClPostMaster.cc          XrdClPostMaster.hh
                              XrdClPostMasterInterfaces.hh
  XrdClChannel.cc             XrdClChannel.hh
  XrdClStream.cc              XrdClStream.hh
  XrdClXRootDTransport.cc     XrdClXRootDTransport.hh
  XrdClInQueue.cc             XrdClInQueue.hh
  XrdClOutQueue.cc            XrdClOutQueue.hh
  XrdClTaskManager.cc         XrdClTaskManager.hh
  XrdClSIDManager.cc          XrdClSIDManager.hh
  XrdClFileSystem.cc          XrdClFileSystem.hh
  XrdClXRootDMsgHandler.cc    XrdClXRootDMsgHandler.hh
                              XrdClBuffer.hh
                              XrdClMessage.hh
  XrdClMessageUtils.cc        XrdClMessageUtils.hh
  XrdClXRootDResponses.cc     XrdClXRootDResponses.hh
                              XrdClRequestSync.hh
  XrdClFile.cc                XrdClFile.hh
  XrdClFileStateHandler.cc    XrdClFileStateHandler.hh
  XrdClCopyProcess.cc         XrdClCopyProcess.hh
  XrdClClassicCopyJob.cc      XrdClClassicCopyJob.hh
  XrdClThirdPartyCopyJob.cc   XrdClThirdPartyCopyJob.hh
  XrdClAsyncSocketHandler.cc  XrdClAsyncSocketHandler.hh
  XrdClChannelHandlerList.cc  XrdClChannelHandlerList.hh
  XrdClForkHandler.cc         XrdClForkHandler.hh
  XrdClCheckSumManager.cc     XrdClCheckSumManager.hh
  XrdClTransportManager.cc    XrdClTransportManager.hh
                              XrdClSyncQueue.hh
  XrdClJobManager.cc          XrdClJobManager.hh
                              XrdClResponseJob.hh
  XrdClFileTimer.cc           XrdClFileTimer.hh
                              XrdClUglyHacks.hh
                              XrdClPlugInInterface.hh
  XrdClPlugInManager.cc       XrdClPlugInManager.hh
                              XrdClPropertyList.hh
                              XrdClCopyJob.hh
  ${LIBEVENT_POLLER_FILES}
)

target_link_libraries(
  XrdCl
  XrdUtils
  pthread
  dl
  ${LIBEVENT_LIB}
  ${LIBEVENTPTHREADS_LIB})

set_target_properties(
  XrdCl
  PROPERTIES
  INTERFACE_LINK_LIBRARIES ""
  LINK_INTERFACE_LIBRARIES ""
  VERSION   ${XRD_CL_VERSION}
  SOVERSION ${XRD_CL_SOVERSION} )

#-------------------------------------------------------------------------------
# xrdfs
#-------------------------------------------------------------------------------
add_executable(
  xrdfs
  XrdClFS.cc
  XrdClFSExecutor.cc         XrdClFSExecutor.hh )

target_link_libraries(
  xrdfs
  pthread
  XrdCl
  ${READLINE_LIBRARY}
  ${NCURSES_LIBRARY} )

#-------------------------------------------------------------------------------
# xrdcopy
#-------------------------------------------------------------------------------
add_executable(
  xrdcopy
  XrdClCopy.cc )

target_link_libraries(
  xrdcopy
  XrdCl
  XrdAppUtils )

#-------------------------------------------------------------------------------
# Install
#-------------------------------------------------------------------------------
install(
  TARGETS XrdCl xrdfs xrdcopy
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR} )

install(
  FILES
    XrdClAnyObject.hh
    XrdClBuffer.hh
    XrdClConstants.hh
    XrdClCopyProcess.hh
    XrdClDefaultEnv.hh
    XrdClEnv.hh
    XrdClFile.hh
    XrdClFileSystem.hh
    XrdClMessage.hh
    XrdClMonitor.hh
    XrdClPostMaster.hh
    XrdClPostMasterInterfaces.hh
    XrdClTransportManager.hh
    XrdClStatus.hh
    XrdClURL.hh
    XrdClXRootDResponses.hh
    XrdClPlugInInterface.hh
    XrdClPlugInManager.hh
    XrdClPropertyList.hh
  DESTINATION    ${CMAKE_INSTALL_INCLUDEDIR}/xrootd/XrdCl )

install(
  FILES
  ${PROJECT_SOURCE_DIR}/docs/man/xrdfs.1
  ${PROJECT_SOURCE_DIR}/docs/man/xrdcopy.1
  DESTINATION ${CMAKE_INSTALL_MANDIR}/man1 )
