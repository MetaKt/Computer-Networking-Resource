# CMakeLists.txt
#
# Wireshark - Network traffic analyzer
# By Gerald Combs <gerald@wireshark.org>
# Copyright 1998 Gerald Combs
#
# SPDX-License-Identifier: GPL-2.0-or-later
#

include(WiresharkPlugin)

# Plugin name and version info (major minor micro extra)
set_module_info(irda 0 0 6 0)

set(DISSECTOR_SRC
	packet-ircomm.c
	packet-irda.c
	packet-sir.c
)

set(PLUGIN_FILES
	plugin.c
	${DISSECTOR_SRC}
)

set_source_files_properties(
	${PLUGIN_FILES}
	PROPERTIES
	COMPILE_FLAGS "${WERROR_COMMON_FLAGS}"
)

register_plugin_files(plugin.c
	plugin
	"IrDA dissection"
	${DISSECTOR_SRC}
)

add_wireshark_epan_plugin_library(irda)

target_link_libraries(irda epan)

install_plugin(irda epan)

file(GLOB DISSECTOR_HEADERS RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" "*.h")
CHECKAPI(
	NAME
	  irda
	SWITCHES
	  --group dissectors-prohibited
	  --group dissectors-restricted
	SOURCES
	  ${DISSECTOR_SRC}
	  ${DISSECTOR_HEADERS}
)

#
# Editor modelines  -  https://www.wireshark.org/tools/modelines.html
#
# Local variables:
# c-basic-offset: 8
# tab-width: 8
# indent-tabs-mode: t
# End:
#
# vi: set shiftwidth=8 tabstop=8 noexpandtab:
# :indentSize=8:tabSize=8:noTabs=false:
#
