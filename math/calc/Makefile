# Created by: jmz
# $FreeBSD$

PORTNAME=	calc
PORTVERSION=	2.12.5.3
CATEGORIES=	math
MASTER_SITES=	SF

MAINTAINER=	johans@FreeBSD.org
COMMENT=	Arbitrary precision calculator

USES=		gmake tar:bzip2 readline
USE_LDCONFIG=	yes

MAKE_JOBS_UNSAFE=	yes

CALCPAGER?=	more
MAKE_ENV+=	CALCPAGER="${CALCPAGER}" CAT="${CAT}" CHMOD="${CHMOD}" \
		CP="${CP}" GMAKE="${MAKE_CMD}" LN="${LN}" MKDIR="${MKDIR}" \
		MV="${MV}" RM="${RM}" SED="${SED}" TOUCH="${TOUCH}" \
		TRUE="${TRUE}"
.if !exists(/usr/lib/libreadline.so)
MAKE_ARGS=	READLINE_INCLUDE="-I${LOCALBASE}/include" \
		READLINE_LIB="-L${LOCALBASE}/lib -lreadline"
.endif

PLIST_SUB+=	VERSION=${PORTVERSION}

regression-test: build
	( cd ${WRKSRC}; ${SETENV} ${MAKE_ENV} ${MAKE_CMD} check )

post-install:
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/bin/calc \
		${STAGEDIR}${PREFIX}/lib/lib*.so

.include <bsd.port.mk>
