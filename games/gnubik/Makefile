# Created by: Dmitry Marakasov <amdmi3@FreeBSD.org>
# $FreeBSD$

PORTNAME=	gnubik
PORTVERSION=	2.4.1
CATEGORIES=	games
MASTER_SITES=	GNU

MAINTAINER=	amdmi3@FreeBSD.org
COMMENT=	3D Rubik's cube game

LICENSE=	GPLv3+
LICENSE_FILE=	${WRKSRC}/COPYING

LIB_DEPENDS=	libguile.so:lang/guile \
		libgtkglext-x11-1.0.so:x11-toolkits/gtkglext

GNU_CONFIGURE=	yes
USES=		gettext gmake localbase pkgconfig
USE_GNOME=	gtk20
USE_GL=		gl glu

PORTDOCS=	AUTHORS ChangeLog NEWS README TODO

INFO=		gnubik

OPTIONS_DEFINE=	DOCS

post-install:
	@${RMDIR} ${STAGEDIR}${PREFIX}/share/icons/hicolor/24x24/apps
	@${RMDIR} ${STAGEDIR}${PREFIX}/share/icons/hicolor/24x24

post-install-DOCS-on:
	@${MKDIR} ${STAGEDIR}${DOCSDIR}
.for f in ${PORTDOCS}
	${INSTALL_DATA} ${WRKSRC}/${f} ${STAGEDIR}${DOCSDIR}/
.endfor

.include <bsd.port.mk>
