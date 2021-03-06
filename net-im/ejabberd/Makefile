# $FreeBSD$

PORTNAME=	ejabberd
PORTVERSION=	16.09
CATEGORIES=	net-im
MASTER_SITES=	http://www.process-one.net/downloads/${PORTNAME}/${PORTVERSION}/:p1 \
		http://distfiles.pirateparty.in/ashish/:deps \
		LOCAL/ashish:deps
DISTFILES+=	${DISTNAME}.tgz:p1 ejabberd-deps-9.tar.xz:deps

MAINTAINER=	ashish@FreeBSD.org
COMMENT=	Free and Open Source distributed fault-tolerant Jabber server

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/COPYING

BUILD_DEPENDS=	erlc:lang/erlang \
		git:devel/git
RUN_DEPENDS=	erl:lang/erlang \
		bash:shells/bash
LIB_DEPENDS=	libexpat.so:textproc/expat2 \
		libyaml.so:textproc/libyaml

USERS=		ejabberd
GROUPS=		${USERS}

GNU_CONFIGURE=	yes
USES=		cpe gmake pkgconfig shebangfix
USE_RC_SUBR=	${PORTNAME}
NOPRECIOUSMAKEVARS=	yes
CPE_VENDOR=	process-one

OPTIONS_DEFINE=	ODBC PAM DOCS MYSQL PGSQL ICONV RIAK ZLIB REDIS SQLITE TOOLS FULLXML #LOGDB
OPTIONS_DEFAULT=	ICONV

EJABBERD_LIBDIR=lib/erlang/lib
MAKE_ENV=	PORTVERSION=${PORTVERSION}
CONFIGURE_ARGS+=--localstatedir=/var --libdir=${PREFIX}/${EJABBERD_LIBDIR}
MAKE_ARGS=	"CPPFLAGS=${CPPFLAGS}" "CFLAGS=${CFLAGS}" "LDFLAGS=${LDFLAGS}"
CPPFLAGS+=	-I${LOCALBASE}/include
CFLAGS+=	-I${LOCALBASE}/include
LDFLAGS+=	-L${LOCALBASE}/lib
PKGMESSAGE=	${WRKDIR}/pkg-message
PKGINSTALL=	${WRKDIR}/pkg-install
PLIST_SUB=	PORTNAME=${PORTNAME} PORTVERSION=${PORTVERSION} \
		EJABBERD_LIBDIR=${EJABBERD_LIBDIR}
SUB_FILES=	pkg-message pkg-install
SUB_LIST=	PORTNAME=${PORTNAME} PORTVERSION=${PORTVERSION} \
		USERS=${USERS} GROUPS=${GROUPS} SHAREMODE=${SHAREMODE} \
		SHAREOWN=${SHAREOWN} SHAREGRP=${SHAREGRP} \
		EJABBERD_LIBDIR=${EJABBERD_LIBDIR}
SHEBANG_FILES=	${WRKSRC}/ejabberdctl.template

OPTIONS_SUB=		yes
RIAK_DESC=	Riak database support
TOOLS_DESC=	Build development tools
FULLXML_DESC=	Full XML support
#LOGDB_DESC=	Build with mod_logdb patch (3rd party)
PAM_CONFIGURE_ENABLE=	pam
ODBC_CONFIGURE_ENABLE=	odbc
MYSQL_CONFIGURE_ENABLE=	mysql
PGSQL_CONFIGURE_ENABLE=	pgsql
RIAK_CONFIGURE_ENABLE=	riak
ICONV_CONFIGURE_ENABLE=	iconv
REDIS_CONFIGURE_ENABLE=	redis
SQLITE_CONFIGURE_ENABLE=sqlite
TOOLS_CONFIGURE_ENABLE=	tools
FULLXML_CONFIGURE_ENABLE=	full-xml
ZLIB_CONFIGURE_ENABLE=	zlib
SQLITE_USES=		sqlite
ICONV_USES=		iconv:wchar_t
ICONV_LDFLAGS=		${ICONV_LIB}
DOCS_MAKE_ARGS_OFF=	NOPORTDOCS=yes
#LOGDB_PATCH_SITES=	http://www.dp.uz.gov.ua/o.palij/mod_logdb/
#LOGDB_PATCHFILES=	patch-mod_logdb-${PORTVERSION}.diff:-p1

USE_OPENSSL=	yes

.include <bsd.port.pre.mk>

post-extract:
	@${LN} -s ${WRKDIR}/deps ${WRKSRC}/

#post-extract-LOGDB-on:
#	@${MV} ${WRKDIR}/deps/p1_mysql ${WRKDIR}/deps/p1_mysql_old
#	@${MV} ${WRKDIR}/deps/p1_mysql_logdb ${WRKDIR}/deps/p1_mysql

pre-configure:
	@${REINPLACE_CMD} -e 's,/usr/local,${LOCALBASE},g' ${WRKSRC}/configure
	@${REINPLACE_CMD} -e 's,%%LOCALBASE%%,${LOCALBASE},g' \
		${WRKSRC}/ejabberdctl.template

pre-install:
	@${MKDIR} ${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR}

post-install:
.if ${PORT_OPTIONS:MPAM}
	@${INSTALL} -m 444 ${FILESDIR}/pam_ejabberd \
		${STAGEDIR}${PREFIX}/etc/pam.d/ejabberd
	@${INSTALL} -m 0750 ${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR}/${PORTNAME}-${PORTVERSION}/lib/p1_pam-1.0.0/priv/bin/epam \
		${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR}/${PORTNAME}-${PORTVERSION}/priv/bin/epam
.endif
	@${FIND} ${STAGEDIR}${PREFIX}/${EJABBERD_LIBDIR} -type f \
		-name '*.so' -exec ${STRIP_CMD} '{}' \;

.include <bsd.port.post.mk>
