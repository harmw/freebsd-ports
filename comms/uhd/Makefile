# $FreeBSD$

PORTNAME=	uhd
PORTVERSION=	3.9.5
PORTREVISION=	2
CATEGORIES=	comms hamradio
MASTER_SITES=	http://files.ettus.com/binaries/images/
DISTFILES=	${IMAGE_FILE}
DIST_SUBDIR=	${PORTNAME}

MAINTAINER=	mr@FreeBSD.org
COMMENT=	Ettus Research UHD driver framework

USE_GITHUB=     yes
GH_ACCOUNT=     EttusResearch
GH_TAGNAME=     32951af2f3e7b1ddb56486c9f15b2e1033605bd6

CONFLICTS=	usrp-[0-9]*

LIB_DEPENDS=	libboost_python.so:devel/boost-python-libs
BUILD_DEPENDS=	${LOCALBASE}/include/boost/tuple/tuple.hpp:devel/boost-libs \
		cheetah-analyze:devel/py-cheetah \
		rst2html:textproc/py-docutils \
		orcc:devel/orc \
		${PYTHON_PKGNAMEPREFIX}mako>0:textproc/py-mako

USES=			compiler:c++0x cmake:outsource gmake pkgconfig dos2unix python:2.7
USE_LDCONFIG=		yes
CMAKE_SOURCE_PATH=	${WRKSRC}/host
#MAKE_JOBS_UNSAFE=	yes
CXXFLAGS+=	-msse2

CMAKE_ARGS+=	-DPKG_LIB_DIR:STRING="share/uhd" -DUHD_TXRX_DEBUG_PRINTS="yes"
# for excruciating debug use this -db
#CMAKE_ARGS+=	--debug-output --trace

IMAGE_FILE=	uhd-images_003.009.005-release.zip

OPTIONS_DEFINE= DOXYGEN
#OPTIONS_DEFAULT= DOXYGEN

OPTIONS_SUB=yes
DOXYGEN_BUILD_DEPENDS=  doxygen:devel/doxygen

.include <bsd.port.pre.mk>

.if ${PORT_OPTIONS:MDOXYGEN}
BUILD_DEPENDS+=	doxygen:devel/doxygen
CMAKE_ARGS+=	-DENABLE_DOXYGEN:STRING="ON"
HAVEDOCS=	YES
.endif

post-patch:
# USES=pathfix handles LIBRARY_INSTALL_DIR but not LIBRARY_DIR
	@${REINPLACE_CMD} '/pkgconfig/s|LIBRARY_DIR}|CMAKE_INSTALL_PREFIX}/libdata|' \
		${PATCH_WRKSRC}/host/CMakeLists.txt

do-install:
# install host component
# hack the install prefix now
	@${REINPLACE_CMD} -e "s|/usr/local|${STAGEDIR}${PREFIX}|g" \
		${CONFIGURE_WRKSRC}/cmake_install.cmake
	cd ${CONFIGURE_WRKSRC} && ${MAKE} install
.for subdir in images
	${CP} -Rp ${WRKDIR}/${IMAGE_FILE:S|.zip||}/share/uhd/${subdir} ${STAGEDIR}${DATADIR}
.endfor

.include <bsd.port.post.mk>
