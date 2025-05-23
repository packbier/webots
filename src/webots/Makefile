# Copyright 1996-2024 Cyberbotics Ltd.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

START := $(shell date +%s)

WEBOTS_PATH = ../..

include $(WEBOTS_PATH)/resources/Makefile.os.include

WEBOTS_DEPENDENCY_PATH ?= $(WEBOTS_PATH)/dependencies

# compiler
ifeq ($(CC),cc)
CC = gcc
endif

# module dependencies: please keep these lists of dependencies up-to-date so that we don't break dependencies in the code
# listed from the module with the less dependencies to the module with the more dependencies

WB_ODE_INCLUDE           = $(QT_CORE_INCLUDE) $(QT_GUI_INCLUDE) $(ODE_INCLUDE) $(WREN_INCLUDE) $(CONTROLLER_INCLUDE) -Iapp -Icore -Imaths -Inodes -Inodes/utils -Ivrml -Iengine
WB_MATHS_INCLUDE         = $(QT_CORE_INCLUDE)
WB_CORE_INCLUDE          = $(QT_CORE_INCLUDE) $(QT_NETWORK_INCLUDE) $(QT_GUI_INCLUDE) $(QT_QML_INCLUDE) -Iqml -Iexternal/compilation_timestamp
WB_UTIL_INCLUDE          = $(QT_CORE_INCLUDE) $(ODE_INCLUDE) -Isound -Icore
WB_WREN_INCLUDE          = $(QT_CORE_INCLUDE) $(QT_GUI_INCLUDE) $(QT_OPENGL_INCLUDE) $(WREN_INCLUDE) -Icore -Imaths -Ivrml -Iutil
WB_VRML_INCLUDE          = $(QT_CORE_INCLUDE) -Icore -Imaths -Ivrml
WB_USER_COMMANDS_INCLUDE = $(QT_CORE_INCLUDE) $(QT_GUI_INCLUDE) $(QT_WIDGETS_INCLUDE) -Icore -isystem external/siphash -Imaths -Inodes -Inodes/utils -Iode -Iuser_commands -Ivrml
WB_WIDGETS_INCLUDE       = $(QT_CORE_INCLUDE) $(QT_GUI_INCLUDE) $(QT_WIDGETS_INCLUDE) -Icore -Imaths -Iuser_commands -Ivrml
WB_EDITOR_INCLUDE        = $(QT_CORE_INCLUDE) $(QT_GUI_INCLUDE) $(QT_WIDGETS_INCLUDE) $(QT_PRINT_SUPPORT_INCLUDE) -Icore -Iwidgets -Imaths -Inodes -Inodes/utils -Iode -Iuser_commands -Ivrml
WB_PLUGINS_INCLUDE       = $(QT_CORE_INCLUDE) $(ODE_INCLUDE) -Icore -Imaths -Inodes -Inodes/utils -Ivrml
WB_ENGINE_INCLUDE        = $(QT_CORE_INCLUDE) $(QT_NETWORK_INCLUDE) $(ODE_INCLUDE) $(WREN_INCLUDE) -Icore -Imaths -Inodes -Inodes/utils -Iplugins -Isound -Iuser_commands -Iutil -Ivrml
WB_CONTROL_INCLUDE       = $(QT_CORE_INCLUDE) $(QT_NETWORK_INCLUDE) -Icore -Iengine -Imaths -Inodes -Inodes/utils -Iode -Ivrml -Iutil
# dependency of NODES on QT_GUI is necessary in WbImageTexture to retrieve the size of a texture before loading it in Wren
WB_NODES_INCLUDE         = $(QT_CORE_INCLUDE) $(QT_NETWORK_INCLUDE) $(QT_GUI_INCLUDE) $(ODE_INCLUDE) $(CONTROLLER_INCLUDE) $(OIS_INCLUDE) $(FREETYPE_INCLUDE) $(WREN_INCLUDE) $(STB_INCLUDE) $(ASSIMP_INCLUDE) -Iapp -Icore -isystem external/siphash -Imaths -Inodes -Inodes/utils -Iode -Iplugins -Isound -Iutil -Ivrml
WB_APP_INCLUDE           = $(QT_CORE_INCLUDE) $(QT_NETWORK_INCLUDE) $(ODE_INCLUDE) $(WREN_INCLUDE) -Icontrol -Icore -Ieditor -Iengine -Imaths -Inodes -Inodes/utils -Iplugins -Iscene_tree -Ivrml
WB_SCENE_TREE_INCLUDE    = $(QT_CORE_INCLUDE) $(QT_GUI_INCLUDE) $(QT_WIDGETS_INCLUDE) $(ASSIMP_INCLUDE) -isystem external/siphash -Iapp -Icore -Ieditor -Imaths -Inodes -Inodes/utils -Iode -Iuser_commands -Ivrml -Iwidgets
WB_SOUND_INCLUDE         = $(QT_CORE_INCLUDE) $(QT_XML_INCLUDE) $(ODE_INCLUDE) $(PICO_INCLUDE) -Icore -Imaths -Inodes -Inodes/utils -Isound -Ivrml
WB_GUI_INCLUDE           = $(QT_CORE_INCLUDE) $(QT_NETWORK_INCLUDE) $(QT_WEBSOCKETS_INCLUDE) $(QT_GUI_INCLUDE) $(QT_WIDGETS_INCLUDE) $(QT_OPENGL_INCLUDE) $(QT_OPENGL_WIDGETS_INCLUDE) $(ODE_INCLUDE) $(WREN_INCLUDE) -Iapp -Icontrol -Icore -Iwidgets -Ieditor -Iengine -isystem external/siphash -Imaths -Inodes -Inodes/utils -Iplugins -Iscene_tree -Isound -Iuser_commands -Iutil -Ivrml
WB_RENDER_INCLUDE        = $(QT_CORE_INCLUDE) $(QT_GUI_INCLUDE) $(QT_OPENGL_INCLUDE) $(QT_WIDGETS_INCLUDE) $(WREN_INCLUDE) -isystem external/siphash

HOSTTYPE ?= $(shell uname -m)

# clear out all suffixes for implicit rules to speed up Makefile
.SUFFIXES:
%:: %,v
%:: RCS/%,v
%:: RCS/%
%:: s.%
%:: SCCS/s.%

OIS_INCLUDE = -isystem $(WEBOTS_PATH)/include/libOIS
PICO_INCLUDE = -I$(WEBOTS_PATH)/include/libpico
WREN_INCLUDE = -I$(WEBOTS_PATH)/include
STB_INCLUDE  = -isystem $(WEBOTS_PATH)/src/stb
OPEN_VR_INCLUDE = -isystem $(WEBOTS_PATH)/include/openvr

ifeq ($(OSTYPE),darwin)
EXECUTABLE_PATH    = $(WEBOTS_PATH)/Contents/MacOS
EXECUTABLE_PATH_SETUP := $(shell mkdir -p $(EXECUTABLE_PATH))
FRAMEWORKS         = $(WEBOTS_PATH)/Contents/Frameworks
TARGET             = $(EXECUTABLE_PATH)/webots
ODE                = $(WEBOTS_PATH)/src/ode
LIB_WREN           = ../wren/libwren.a ../glad/libglad.a
QT_CORE_INCLUDE    = -isystem $(FRAMEWORKS)/QtCore.framework/Headers
QT_GUI_INCLUDE     = -isystem $(FRAMEWORKS)/QtGui.framework/Headers
QT_WIDGETS_INCLUDE = -isystem $(FRAMEWORKS)/QtWidgets.framework/Headers
QT_OPENGL_INCLUDE  = -isystem $(FRAMEWORKS)/QtOpenGL.framework/Headers
QT_OPENGL_WIDGETS_INCLUDE = -isystem $(FRAMEWORKS)/QtOpenGLWidgets.framework/Headers
QT_PRINT_SUPPORT_INCLUDE = -isystem $(FRAMEWORKS)/QtPrintSupport.framework/Headers
QT_QML_INCLUDE     = -isystem $(FRAMEWORKS)/QtQml.framework/Headers
QT_NETWORK_INCLUDE = -isystem $(FRAMEWORKS)/QtNetwork.framework/Headers
QT_WEBSOCKETS_INCLUDE = -isystem $(FRAMEWORKS)/QtWebSockets.framework/Headers
QT_XML_INCLUDE     = -isystem $(FRAMEWORKS)/QtXml.framework/Headers
FREETYPE_INCLUDE   = -isystem $(WEBOTS_DEPENDENCY_PATH)/freetype2
ASSIMP_INCLUDE     = -I$(WEBOTS_DEPENDENCY_PATH)/assimp/include
LD_FLAGS           = -mmacosx-version-min=$(MACOSX_MIN_SDK_VERSION) -L$(WEBOTS_LIB_PATH) -lOIS -lpico -lassimp -Xlinker -rpath -Xlinker @loader_path/../..
MOC                =  $(WEBOTS_PATH)/bin/qt/moc
LIBS_FRAMEWORKS    = -F$(FRAMEWORKS) -framework OpenAL -framework OpenGL -framework QtCore -framework QtGui -framework QtPrintSupport -framework QtQml -framework QtNetwork -framework QtOpenGL -framework QtOpenGLWidgets -framework QtWebSockets -framework QtWidgets -framework QtXml -framework IOKit -framework CoreFoundation -framework Cocoa
LIBS              += -bind_at_load $(LIBS_FRAMEWORKS) $(LIB_WREN)
CFLAGS             = -isysroot $(MACOSX_SDK_PATH) -mmacosx-version-min=$(MACOSX_MIN_SDK_VERSION)
CXXFLAGS           = -std=c++11 -F$(FRAMEWORKS) -x objective-c++
EXTRA_CMD          = cp launcher/webots-mac.sh $(WEBOTS_PATH)/webots && chmod 755 $(WEBOTS_PATH)/webots
FILES_TO_REMOVE    = $(WEBOTS_PATH)/webots
ifeq ($(MAKECMDGOALS), profile)
LIB_ODE = ../ode/libode.a
LIBS += $(LIB_ODE)
LD_FLAGS += -lpthread
else
LD_FLAGS += -lode
endif
# To compile with libpico without warnings about the "register" keyword.
CFLAGS += -Wno-deprecated-register

else ifeq ($(OSTYPE),linux)
LIB_WREN           = ../wren/libwren.a ../glad/libglad.a
ASSIMP_INCLUDE     = -I$(WEBOTS_PATH)/include/libassimp/include
QT_CORE_INCLUDE    = -isystem $(WEBOTS_PATH)/include/qt/QtCore
QT_GUI_INCLUDE     = -isystem $(WEBOTS_PATH)/include/qt/QtGui
QT_WIDGETS_INCLUDE = -isystem $(WEBOTS_PATH)/include/qt/QtWidgets
QT_PRINT_SUPPORT_INCLUDE = -isystem $(WEBOTS_PATH)/include/qt/QtPrintSupport
QT_QML_INCLUDE     = -isystem $(WEBOTS_PATH)/include/qt/QtQml
QT_OPENGL_INCLUDE  = -isystem $(WEBOTS_PATH)/include/qt/QtOpenGL
QT_OPENGL_WIDGETS_INCLUDE = -isystem $(WEBOTS_PATH)/include/qt/QtOpenGLWidgets
QT_NETWORK_INCLUDE = -isystem $(WEBOTS_PATH)/include/qt/QtNetwork
QT_WEBSOCKETS_INCLUDE = -isystem $(WEBOTS_PATH)/include/qt/QtWebSockets
QT_XML_INCLUDE     = -isystem $(WEBOTS_PATH)/include/qt/QtXml
FREETYPE_INCLUDE   = -I/usr/include/freetype2
TARGET             = $(WEBOTS_PATH)/bin/webots-bin
CFLAGS             = -fPIC
CXXFLAGS           = -std=c++11
MOC                = $(WEBOTS_PATH)/bin/qt/moc
LIBS              += $(LIB_WREN) -Wl,-rpath $(WEBOTS_LIB_PATH) -L$(WEBOTS_LIB_PATH) -lQt6Core -lQt6Network -lQt6Gui -lQt6OpenGL -lQt6OpenGLWidgets -lQt6WebSockets -lQt6Widgets -lQt6PrintSupport -lQt6Qml -lQt6Xml -lopenal -lGL -lGLU -ldl -lOIS -lpico -lfreetype -lassimp -lrt
LD_FLAGS           = -rdynamic
EXTRA_CMD          = cp launcher/webots-linux.sh $(WEBOTS_PATH)/webots && chmod 755 $(WEBOTS_PATH)/webots
FILES_TO_REMOVE    = $(WEBOTS_PATH)/webots
ifeq ($(MAKECMDGOALS), profile)
LIB_ODE = ../ode/libode.a
LIBS += $(LIB_ODE) -lpthread
else
LIBS += -lode -lpthread
endif

else ifeq ($(OSTYPE),windows)
vpath %.c ../controller/c
WB_NODES_INCLUDE         += $(OPEN_VR_INCLUDE)
C_SOURCES                += system.c
LIB_WREN                  = ../wren/wren.a ../glad/glad.a
QT_CORE_INCLUDE           = -isystem $(WEBOTS_PATH)/include/qt/QtCore
QT_GUI_INCLUDE            = -isystem $(WEBOTS_PATH)/include/qt/QtGui
QT_WIDGETS_INCLUDE        = -isystem $(WEBOTS_PATH)/include/qt/QtWidgets
QT_PRINT_SUPPORT_INCLUDE  = -isystem $(WEBOTS_PATH)/include/qt/QtPrintSupport
QT_QML_INCLUDE            = -isystem $(WEBOTS_PATH)/include/qt/QtQml
QT_OPENGL_INCLUDE         = -isystem $(WEBOTS_PATH)/include/qt/QtOpenGL
QT_OPENGL_WIDGETS_INCLUDE = -isystem $(WEBOTS_PATH)/include/qt/QtOpenGLWidgets
QT_NETWORK_INCLUDE        = -isystem $(WEBOTS_PATH)/include/qt/QtNetwork
QT_WEBSOCKETS_INCLUDE     = -isystem $(WEBOTS_PATH)/include/qt/QtWebSockets
QT_XML_INCLUDE            = -isystem $(WEBOTS_PATH)/include/qt/QtXml
ASSIMP_INCLUDE            = -I$(WEBOTS_PATH)/dependencies
FREETYPE_INCLUDE          = -I/mingw64/include/freetype2
TARGET_PATH               = $(WEBOTS_PATH)/msys64/mingw64/bin
TARGET                    = $(TARGET_PATH)/webots-bin.exe
CFLAGS                    = -isystem /mingw64/lib/libzip/include
CXXFLAGS                  = -std=c++11 -D_USE_MATH_DEFINES
MOC                       = /mingw64/share/qt6/bin/moc
MOC_FLAGS                += -D_WIN32
LIBS                     += $(LIB_WREN) -L$(TARGET_PATH) -L/mingw64/bin -L/mingw64/lib -lQt6Core -lQt6Network -lQt6Gui -lQt6OpenGL -lQt6OpenGLWidgets -lQt6WebSockets -lQt6Widgets -lQt6PrintSupport -lQt6Qml -lQt6Xml -lode -lopenal -lopengl32 -liphlpapi -ld3d9 -lgdi32 -lglu32 -lOIS -ldinput8 -ldxguid -lole32 -lsapi -loleaut32 -luuid -lpico -lfreetype-6 -lopenvr_api -lassimp-5
LD_FLAGS                  = -Wl,--enable-auto-import
EXTRA_CMD                 = cp launcher/webots-bin.exe.config $(TARGET_PATH)/
FILES_TO_REMOVE           = $(TARGET_PATH)/webots-bin.exe.config
ifeq ($(MAKECMDGOALS),release)
# -fipa-cp-clone included in -O3 seems to cause some stack overflow on Windows,
# in particular in WbController::setProcessEnvironment(), however increasing the
# stack size doesn't seems to help, so we need to remove this optimization
CFLAGS                   += -fno-ipa-cp-clone
LD_FLAGS                 += -Wl,-subsystem,windows
else
EXTRA_CMD                += ; cp launcher/webots_debug.bat $(TARGET_PATH)/webots_debug.bat
endif
ifeq ($(MAKECMDGOALS), profile)
LIB_ODE = ../ode/ode.a
LIBS += $(LIB_ODE) -lpthread
else
LIBS += -lode
endif
endif

ALL_INCLUDE        = -Iapp -Icontrol -Icore -Iwidgets -Ieditor -Iengine -isystem external/siphash -Igui -Imaths -Inodes -Inodes/utils -Iplugins -Iscene_tree -Isound -Iuser_commands -Ivrml -Iutil -Iexternal/compilation_timestamp

CONTROLLER_INCLUDE = -I$(WEBOTS_PATH)/include/controller/c
ODE_INCLUDE        = -Iode -isystem $(WEBOTS_PATH)/include/ode
QT_CORE_INCLUDE   += -I$(OBJDIR)
WREN_INCLUDE      += -Iwren
INCLUDE            = $(ALL_INCLUDE) $(CONTROLLER_INCLUDE) $(ODE_INCLUDE) $(QT_CORE_INCLUDE) $(QT_GUI_INCLUDE) $(QT_WIDGETS_INCLUDE) $(QT_PRINT_SUPPORT_INCLUDE) $(QT_OPENGL_INCLUDE) $(QT_NETWORK_INCLUDE) $(OIS_INCLUDE) $(PICO_INCLUDE) $(WREN_INCLUDE) $(FREETYPE_INCLUDE) $(STB_INCLUDE) $(ASSIMP_INCLUDE)

ifeq ($(MAKECMDGOALS),debug)
CFLAGS += -ggdb
endif

ifeq ($(TREAT_WARNINGS_AS_ERRORS),1)
CFLAGS += -Werror
endif

ifeq ($(MAKECMDGOALS),profile)
CFLAGS += -pg -O3 -DNDEBUG
ifneq ($(OSTYPE),darwin)
LD_FLAGS += -pg
endif
endif

ifeq ($(MAKECMDGOALS),release)
CFLAGS += -O3 -DNDEBUG
ifneq ($(OSTYPE),darwin)
LD_FLAGS += -s
endif
endif

OBJDIR=build/$(MAKECMDGOALS)
ifneq (,$(findstring $(MAKECMDGOALS),debug profile release))
ifeq ($(OSTYPE),darwin)
BUILD_PATH_SETUP := $(shell mkdir -p $(OBJDIR) $(OBJDIR)/x86_64 $(OBJDIR)/arm64)
else
BUILD_PATH_SETUP := $(shell mkdir -p $(OBJDIR))
endif
endif

CFLAGS += -Wall -Wpointer-arith -Wcast-align -Wwrite-strings
CXXFLAGS += -std=c++17 -DQT_NO_DEBUG

ifneq ($(OSTYPE),darwin) # Clang has these warnings by default
GCC_VER_GREATER_OR_EQUAL_TO_5_DOT_4 := $(shell expr `gcc -dumpversion | sed -e 's/\.\([0-9][0-9]\)/\1/g' -e 's/\.\([0-9]\)/0\1/g' -e 's/^[0-9]\{3,4\}$$/&00/'` \>= 50400)
ifeq ($(GCC_VER_GREATER_OR_EQUAL_TO_5_DOT_4),1)
CXXFLAGS += -Wsuggest-override
endif
endif

# objects that inherit from QObject (alphabetical order)
QT_SOURCES = WbAboutBox.cpp \
  WbAbstractAppearance.cpp \
  WbAbstractDragEvent.cpp \
  WbAbstractCamera.cpp \
  WbAccelerometer.cpp \
  WbActionManager.cpp \
  WbAddInertiaMatrixDialog.cpp \
  WbAddNodeDialog.cpp \
  WbAltimeter.cpp \
  WbAnimationRecorder.cpp \
  WbAppearance.cpp \
  WbApplication.cpp \
  WbBackground.cpp \
  WbBallJoint.cpp \
  WbBallJointParameters.cpp \
  WbBaseNode.cpp \
  WbBasicJoint.cpp \
  WbBillboard.cpp \
  WbBoolEditor.cpp \
  WbBox.cpp \
  WbBrake.cpp \
  WbBuildEditor.cpp \
  WbCadShape.cpp \
  WbCamera.cpp \
  WbCapsule.cpp \
  WbCharger.cpp \
  WbColor.cpp \
  WbColorEditor.cpp \
  WbCompass.cpp \
  WbCone.cpp \
  WbConnector.cpp \
  WbConsole.cpp \
  WbContactPointsRepresentation.cpp \
  WbContactProperties.cpp \
  WbControlledWorld.cpp \
  WbController.cpp \
  WbCoordinate.cpp \
  WbCoordinateSystem.cpp \
  WbCylinder.cpp \
  WbDamping.cpp \
  WbDirectionalLight.cpp \
  WbDistanceSensor.cpp \
  WbDisplay.cpp \
  WbDockTitleBar.cpp \
  WbDockWidget.cpp \
  WbDoubleEditor.cpp \
  WbDoubleSpinBox.cpp \
  WbDownloadManager.cpp \
  WbDownloader.cpp \
  WbDragPoseEvent.cpp \
  WbDragResizeEvent.cpp \
  WbDragSolidEvent.cpp \
  WbElevationGrid.cpp \
  WbEmitter.cpp \
  WbExtendedStringEditor.cpp \
  WbExternProtoEditor.cpp \
  WbInsertExternProtoDialog.cpp \
  WbField.cpp \
  WbFieldChecker.cpp \
  WbFieldDoubleSpinBox.cpp \
  WbFieldEditor.cpp \
  WbFieldIntSpinBox.cpp \
  WbFieldLineEdit.cpp \
  WbFindReplaceDialog.cpp \
  WbFieldValueRestriction.cpp \
  WbFluid.cpp \
  WbFocus.cpp \
  WbFog.cpp \
  WbGeometry.cpp \
  WbGps.cpp \
  WbGroup.cpp \
  WbGuiApplication.cpp \
  WbGuidedTour.cpp \
  WbGuiRefreshOracle.cpp \
  WbGyro.cpp \
  WbHingeJoint.cpp \
  WbHingeJointParameters.cpp \
  WbHinge2Joint.cpp \
  WbImageTexture.cpp \
  WbImmersionProperties.cpp \
  WbIntEditor.cpp \
  WbIntSpinBox.cpp \
  WbIndexedFaceSet.cpp \
  WbIndexedLineSet.cpp \
  WbInertialUnit.cpp \
  WbJoint.cpp \
  WbJointDevice.cpp \
  WbJointParameters.cpp \
  WbJoystickInterface.cpp \
  WbJoystickListener.cpp \
  WbLed.cpp \
  WbLens.cpp \
  WbLensFlare.cpp \
  WbLidar.cpp \
  WbLight.cpp \
  WbLightSensor.cpp \
  WbLinearMotor.cpp \
  WbLineEdit.cpp \
  WbLog.cpp \
  WbMainWindow.cpp \
  WbMassChecker.cpp \
  WbMaterial.cpp \
  WbMatter.cpp \
  WbMesh.cpp \
  WbMFBool.cpp \
  WbMFColor.cpp \
  WbMFDouble.cpp \
  WbMFInt.cpp \
  WbMFNode.cpp \
  WbMFRotation.cpp \
  WbMFString.cpp \
  WbMFVector2.cpp \
  WbMFVector3.cpp \
  WbMicrophone.cpp \
  WbMotor.cpp \
  WbMouse.cpp \
  WbMultipleValue.cpp \
  WbMuscle.cpp \
  WbNewControllerWizard.cpp \
  WbNewPhysicsPluginWizard.cpp \
  WbNewProjectWizard.cpp \
  WbNewProtoWizard.cpp \
  WbNewWorldWizard.cpp \
  WbNewVersionDialog.cpp \
  WbNode.cpp \
  WbNodeEditor.cpp \
  WbNodeOperations.cpp \
  WbNodePane.cpp \
  WbNodeReader.cpp \
  WbNormal.cpp \
  WbOdeContext.cpp \
  WbOpenSampleWorldDialog.cpp \
  WbPaintTexture.cpp \
  WbPbrAppearance.cpp \
  WbPen.cpp \
  WbPhysics.cpp \
  WbPhysicsPlugin.cpp \
  WbPhysicsVectorRepresentation.cpp \
  WbPhysicsViewer.cpp \
  WbPlane.cpp \
  WbPlugin.cpp \
  WbPointLight.cpp \
  WbPointSet.cpp \
  WbPositionSensor.cpp \
  WbPositionViewer.cpp \
  WbPose.cpp \
  WbPreferences.cpp \
  WbPreferencesDialog.cpp \
  WbPropeller.cpp \
  WbProject.cpp \
  WbProjectRelocationDialog.cpp \
  WbProtoIcon.cpp \
  WbProtoManager.cpp \
  WbProtoModel.cpp \
  WbProtoTemplateEngine.cpp \
  WbProtoTreeItem.cpp \
  WbQjsFile.cpp \
  WbRadar.cpp \
  WbRadio.cpp \
  WbRadioPlugin.cpp \
  WbRangeFinder.cpp \
  WbReceiver.cpp \
  WbRecentFilesList.cpp \
  WbRecognition.cpp \
  WbRenderingDevice.cpp \
  WbRenderingDeviceWindow.cpp \
  WbRenderingDeviceWindowFactory.cpp \
  WbResizeManipulator.cpp \
  WbRotationEditor.cpp \
  WbRobot.cpp \
  WbRobotWindow.cpp \
  WbRotationalMotor.cpp \
  WbSaveWarningDialog.cpp \
  WbSceneTree.cpp \
  WbSceneTreeModel.cpp \
  WbSelection.cpp \
  WbSensor.cpp \
  WbSFBool.cpp \
  WbSFColor.cpp \
  WbSFDouble.cpp \
  WbSFInt.cpp \
  WbSFNode.cpp \
  WbSFRotation.cpp \
  WbSFString.cpp \
  WbSFVector2.cpp \
  WbSFVector3.cpp \
  WbShape.cpp \
  WbShareWindow.cpp \
  WbSimulationState.cpp \
  WbSimulationStateIndicator.cpp \
  WbSimulationView.cpp \
  WbSimulationWorld.cpp \
  WbSingleValue.cpp \
  WbSingleTaskApplication.cpp \
  WbSkin.cpp \
  WbSliderJoint.cpp \
  WbSlot.cpp \
  WbSolid.cpp \
  WbSolidMerger.cpp \
  WbSolidReference.cpp \
  WbSpeaker.cpp \
  WbSplashScreen.cpp \
  WbSphere.cpp \
  WbSpotLight.cpp \
  WbStringEditor.cpp \
  WbTcpServer.cpp \
  WbSupervisorUtilities.cpp \
  WbSyntaxHighlighter.cpp \
  WbTelemetry.cpp \
  WbTemplateEngine.cpp \
  WbTemplateManager.cpp \
  WbTextBuffer.cpp \
  WbTextEditor.cpp \
  WbTextFind.cpp \
  WbTextureCoordinate.cpp \
  WbTextureTransform.cpp \
  WbTouchSensor.cpp \
  WbTrack.cpp \
  WbTrackWheel.cpp \
  WbTransform.cpp \
  WbTranslateRotateManipulator.cpp \
  WbTreeItem.cpp \
  WbTreeView.cpp \
  WbTriangleMeshGeometry.cpp \
  WbUndoStack.cpp \
  WbUpdatedDialog.cpp \
  WbValue.cpp \
  WbValueEditor.cpp \
  WbVariant.cpp \
  WbVector2Editor.cpp \
  WbVector3Editor.cpp \
  WbVelocityViewer.cpp \
  WbVideoRecorder.cpp \
  WbVideoRecorderDialog.cpp \
  WbMultimediaStreamingServer.cpp \
  WbVacuumGripper.cpp \
  WbViewpoint.cpp \
  WbView3D.cpp \
  WbVisualBoundingSphere.cpp \
  WbWebotsUpdateDialog.cpp \
  WbWebotsUpdateManager.cpp \
  WbWorld.cpp \
  WbWorldInfo.cpp \
  WbWrenAbstractManipulator.cpp \
  WbWrenAbstractResizeManipulator.cpp \
  WbWrenCamera.cpp \
  WbWrenOpenGlContext.cpp \
  WbWrenFullScreenOverlay.cpp \
  WbWrenRenderingContext.cpp \
  WbWrenTextureOverlay.cpp \
  WbWrenWindow.cpp \
  WbW3dStreamingServer.cpp \
  WbZoom.cpp

# objects that don't inherit from QObject (alphabetical order)
OTHER_SOURCES = \
  main.cpp \
  WbAbstractPose.cpp \
  WbAddItemCommand.cpp \
  WbAffinePlane.cpp \
  WbApplicationInfo.cpp \
  WbBinaryIncubator.cpp \
  WbBoundingSphere.cpp \
  WbClipboard.cpp \
  WbConcreteNodeFactory.cpp \
  WbContactSoundManager.cpp \
  WbContactSound.cpp \
  WbContextMenuGenerator.cpp \
  WbControllerPlugin.cpp \
  WbDataPacket.cpp \
  WbDataStream.cpp \
  WbDevice.cpp \
  WbDesktopServices.cpp \
  WbDictionary.cpp \
  WbDisplayFont.cpp \
  WbDragOverlayEvent.cpp \
  WbDragViewpointEvent.cpp \
  WbDragScaleEvent.cpp \
  WbEditCommand.cpp \
  WbFieldModel.cpp \
  WbFileUtil.cpp \
  WbHiddenKinematicParameters.cpp \
  WbHttpReply.cpp \
  WbIniParser.cpp \
  WbKinematicDifferentialWheels.cpp \
  WbLanguage.cpp \
  WbLanguageTools.cpp \
  WbLightRepresentation.cpp \
  WbLogicalDevice.cpp \
  WbLookupTable.cpp \
  WbMacAddress.cpp \
  WbMatrix3.cpp \
  WbMatrix4.cpp \
  WbMathsUtilities.cpp \
  WbMessageBox.cpp \
  WbMotorSoundManager.cpp \
  WbMultimediaStreamingLimiter.cpp \
  WbNetwork.cpp \
  WbNodeFactory.cpp \
  WbNodeModel.cpp \
  WbNodeProtoInfo.cpp \
  WbNodeUtilities.cpp \
  WbObjectDetection.cpp \
  WbOdeDebugger.cpp \
  WbOdeUtilities.cpp \
  WbParser.cpp \
  WbPerformanceLog.cpp \
  WbPerspective.cpp \
  WbPicoTextToSpeech.cpp \
  WbPolygon.cpp \
  WbPrecision.cpp \
  WbRandom.cpp \
  WbRay.cpp \
  WbResetCommand.cpp \
  WbResizeCommand.cpp \
  WbResizeAndTranslateCommand.cpp \
  WbRotation.cpp \
  WbSimulationCluster.cpp \
  WbSolidDevice.cpp \
  WbSolidUtilities.cpp \
  WbSoundClip.cpp \
  WbSoundEngine.cpp \
  WbSoundSource.cpp \
  WbSpotLightRepresentation.cpp \
  WbStandardPaths.cpp \
  WbSupportPolygonRepresentation.cpp \
  WbSysInfo.cpp \
  WbTesselator.cpp \
  WbToken.cpp \
  WbTokenizer.cpp \
  WbTranslator.cpp \
  WbTriangleMesh.cpp \
  WbTriangleMeshCache.cpp \
  WbUrl.cpp \
  WbVector4.cpp \
  WbVersion.cpp \
  WbVrmlNodeUtilities.cpp \
  WbWaveFile.cpp \
  WbWheelEvent.cpp \
  WbWrenAbstractPostProcessingEffect.cpp \
  WbWrenBloom.cpp \
  WbWrenColorNoise.cpp \
  WbWrenDepthOfField.cpp \
  WbWrenGtao.cpp \
  WbWrenHdr.cpp \
  WbWrenLabelOverlay.cpp \
  WbWrenLensDistortion.cpp \
  WbWrenLensFlare.cpp \
  WbWrenMeshBuffers.cpp \
  WbWrenMotionBlur.cpp \
  WbWrenNoiseMask.cpp \
  WbWrenPicker.cpp \
  WbWrenPostProcessingEffects.cpp \
  WbWrenRangeNoise.cpp \
  WbWrenRangeQuantization.cpp \
  WbWrenShaders.cpp \
  WbWrenSmaa.cpp \
  WbWrenVertexArrayFrameListener.cpp \
  WbWriter.cpp

ifeq ($(OSTYPE),windows)
QT_SOURCES += \
  WbVirtualRealityHeadset.cpp
OTHER_SOURCES += \
  WbMicrosoftTextToSpeech.cpp \
  WbWindowsRegistry.cpp
else
OTHER_SOURCES += \
  WbPosixMemoryMappedFile.cpp
endif

MOC_FILES=$(addprefix $(OBJDIR)/,$(notdir $(QT_SOURCES:%.cpp=%.moc.cpp)))
SOURCES=$(C_SOURCES) $(QT_SOURCES) $(OTHER_SOURCES) $(MOC_FILES)
OBJECTS=$(notdir $(C_SOURCES:.c=.o) $(QT_SOURCES:.cpp=.o) $(OTHER_SOURCES:.cpp=.o) $(MOC_FILES:.cpp=.o))

ifeq ($(OSTYPE),windows)
OBJECTS += webots.o
LAUNCHER = ../../msys64/mingw64/bin/webots.exe
LAUNCHERW = ../../msys64/mingw64/bin/webotsw.exe
endif

# prevent 'make' from deleting these intermediate files
.PRECIOUS: $(MOC_FILES)

vpath %.o $(OBJDIR)
vpath %.hpp app control core widgets editor engine external/siphash gui maths nodes nodes/utils ode wren plugins scene_tree sound user_commands vrml
vpath %.cpp app control core widgets editor engine external/siphash gui maths nodes nodes/utils ode wren plugins scene_tree sound user_commands vrml $(OBJDIR)

default:
	@echo "Type one of the following (OSTYPE is \""$(OSTYPE)"\"):"
	@echo "  make debug                 for debug (with gdb symbols)"
	@echo "  make profile               for profiling (with gprof information)"
	@echo "  make release               for release"
	@echo "  make clean                 remove build directory"
	@echo "  make count-lines           count the lines of code of Webots"

ifneq (,$(filter $(MAKECMDGOALS),debug release profile))
-include $(addprefix $(OBJDIR)/,$(notdir $(SOURCES:.cpp=.d)))
endif

.PHONY: debug profile release clean count-lines

debug profile release: $(TARGET) $(LAUNCHER) $(LAUNCHERW)

ifeq ($(OSTYPE),darwin)
X86_64_OBJECTS = $(addprefix $(OBJDIR)/x86_64/, $(OBJECTS))
ARM64_OBJECTS = $(addprefix $(OBJDIR)/arm64/, $(OBJECTS))

$(TARGET): $(OBJDIR)/x86_64/webots $(OBJDIR)/arm64/webots
	@echo "# creating fat " $@
	@lipo -create -output webots $(OBJDIR)/x86_64/webots $(OBJDIR)/arm64/webots
	@codesign -s - webots
	@mv webots $(TARGET)
	@$(EXTRA_CMD)
	@echo "# webots compiled in `expr \`date +%s\` - $(START)` seconds"

$(OBJDIR)/x86_64/webots: $(X86_64_OBJECTS) $(LIB_WREN) $(LIB_ODE)
	@echo "# linking " $@ "(x64_64)"
	@$(CXX) -target x86_64-apple-macos12 $(LD_FLAGS) -o $@ $(X86_64_OBJECTS) $(LIBS)

$(OBJDIR)/arm64/webots: $(ARM64_OBJECTS) $(LIB_WREN) $(LIB_ODE)
	@echo "# linking " $@ "(arm64)"
	@$(CXX) -target arm64-apple-macos12 $(LD_FLAGS) -o $@ $(ARM64_OBJECTS) $(LIBS)

else

$(TARGET): $(OBJECTS) $(LIB_WREN) $(LIB_ODE)
	@echo "# linking " $@
	@$(CXX) $(LD_FLAGS) -o $(TARGET) $(addprefix $(OBJDIR)/, $(OBJECTS)) $(LIBS)
	@$(EXTRA_CMD)
	@echo "# webots compiled in `expr \`date +%s\` - $(START)` seconds"

endif

$(OBJDIR)/%.d: %.cpp
	@echo "# updating" $@
	@$(CXX) $(CFLAGS) $(CXXFLAGS) $(INCLUDE) -MM -w $< >> $@

$(OBJDIR)/%.moc.cpp: %.hpp
	@echo "# moc-generating" $@
	@$(MOC) $(MOC_FLAGS) $(INCLUDE:-isystem:-I) $< -o $@

$(OBJDIR)/x86_64/%.moc.cpp: %.hpp
	@echo "# moc-generating" $@ "(x86_64)"
	@$(MOC) -target x86_64-apple-macos-11 $(MOC_FLAGS) $(INCLUDE:-isystem:-I) $< -o $@

$(OBJDIR)/arm64/%.moc.cpp: %.hpp
	@echo "# moc-generating" $@ "(arm64)"
	@$(MOC) -target arm64-apple-macos-11 $(MOC_FLAGS) $(INCLUDE:-isystem:-I) $< -o $@

launcher/launcher.o: launcher/launcher.rc
	@windres -i $< -o $@ --define _WIN32 --define __WIN32__ --define __WIN95__ --define __GNUWIN32__

$(LAUNCHERW): launcher/launcher.c launcher/launcher.o
	@echo "# compiling webotsw.exe launcher"
	@gcc $(CFLAGS) -mwindows -DWEBOTSW launcher/launcher.o launcher/launcher.c -lshlwapi -o $@

$(LAUNCHER): launcher/launcher.c
	@echo "# compiling webots.exe launcher"
	@gcc $(CFLAGS) launcher/launcher.c -lshlwapi -o $@

webots.o:gui/webots.rc gui/webots.ico gui/manifest.xml
	@echo "# compiling gui/"$@
	@windres -i $< -o $(OBJDIR)/$@ --define _WIN32 --define __WIN32__ --define __WIN95__ --define __GNUWIN32__

%.o: %.c
	@echo "# compiling" $<
	@$(CC) -c $(CFLAGS) $(CONTROLLER_INCLUDE) $< -o $(OBJDIR)/$@

%.o: $(OBJDIR)/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(INCLUDE) $< -o $(OBJDIR)/$@

%.o: app/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_APP_INCLUDE) $< -o $(OBJDIR)/$@

%.o: control/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_CONTROL_INCLUDE) $< -o $(OBJDIR)/$@

%.o: core/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_CORE_INCLUDE) $< -o $(OBJDIR)/$@

%.o: widgets/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_WIDGETS_INCLUDE) $< -o $(OBJDIR)/$@

%.o: editor/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_EDITOR_INCLUDE) $< -o $(OBJDIR)/$@

%.o: engine/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_ENGINE_INCLUDE) $< -o $(OBJDIR)/$@

%.o: external/siphash/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) -isystem external/siphash $< -o $(OBJDIR)/$@

%.o: gui/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_GUI_INCLUDE) $< -o $(OBJDIR)/$@

%.o: nodes/utils/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_NODES_INCLUDE) $< -o $(OBJDIR)/$@

%.o: nodes/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_NODES_INCLUDE) $< -o $(OBJDIR)/$@

%.o: maths/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_MATHS_INCLUDE) $< -o $(OBJDIR)/$@

%.o: ode/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_ODE_INCLUDE) $< -o $(OBJDIR)/$@

%.o: wren/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_WREN_INCLUDE) $< -o $(OBJDIR)/$@

%.o: util/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_UTIL_INCLUDE) $< -o $(OBJDIR)/$@

%.o: plugins/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_PLUGINS_INCLUDE) $< -o $(OBJDIR)/$@

%.o: scene_tree/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_SCENE_TREE_INCLUDE) $< -o $(OBJDIR)/$@

%.o: sound/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_SOUND_INCLUDE) $< -o $(OBJDIR)/$@

%.o: user_commands/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_USER_COMMANDS_INCLUDE) $< -o $(OBJDIR)/$@

%.o: vrml/%.cpp
	@echo "# compiling" $<
	@$(CXX) -c $(CFLAGS) $(CXXFLAGS) $(WB_VRML_INCLUDE) $< -o $(OBJDIR)/$@

$(OBJDIR)/x86_64/%.o: $(OBJDIR)/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: app/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_APP_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: control/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_CONTROL_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: core/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_CORE_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: widgets/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_WIDGETS_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: editor/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_EDITOR_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: engine/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_ENGINE_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: external/siphash/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) -isystem external/siphash $< -o $@

$(OBJDIR)/x86_64/%.o: gui/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_GUI_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: nodes/utils/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_NODES_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: nodes/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_NODES_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: maths/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_MATHS_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: ode/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_ODE_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: wren/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_WREN_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: util/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_UTIL_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: plugins/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_PLUGINS_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: scene_tree/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_SCENE_TREE_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: sound/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_SOUND_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: user_commands/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_USER_COMMANDS_INCLUDE) $< -o $@

$(OBJDIR)/x86_64/%.o: vrml/%.cpp
	@echo "# compiling" $< "(x86_64)"
	@$(CXX) -target x86_64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_VRML_INCLUDE) $< -o $@


$(OBJDIR)/arm64/%.o: $(OBJDIR)/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: app/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_APP_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: control/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_CONTROL_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: core/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_CORE_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: widgets/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_WIDGETS_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: editor/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_EDITOR_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: engine/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_ENGINE_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: external/siphash/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) -isystem external/siphash $< -o $@

$(OBJDIR)/arm64/%.o: gui/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_GUI_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: nodes/utils/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_NODES_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: nodes/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_NODES_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: maths/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_MATHS_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: ode/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_ODE_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: wren/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_WREN_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: util/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_UTIL_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: plugins/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_PLUGINS_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: scene_tree/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_SCENE_TREE_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: sound/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_SOUND_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: user_commands/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_USER_COMMANDS_INCLUDE) $< -o $@

$(OBJDIR)/arm64/%.o: vrml/%.cpp
	@echo "# compiling" $< "(arm64)"
	@$(CXX) -target arm64-apple-macos12 -c $(CFLAGS) $(CXXFLAGS) $(WB_VRML_INCLUDE) $< -o $@


clean:
	@echo "# cleaning"
	@rm -rf $(TARGET) $(FILES_TO_REMOVE) $(LAUNCHER) $(LAUNCHERW) build launcher/launcher.o

count-lines:
	wc -l */*.c* */*.h*
