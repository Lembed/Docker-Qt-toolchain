FROM ubuntu:14.04

RUN apt-get update
RUN apt-get install -y --no-install-recommends \
	autoconf \
	automake \
	autopoint \
	binutils \
	bison \
	build-essential \
	ca-certificates \
	cmake \
	debhelper \
	devscripts \
	fakeroot \
	flex \
	gcc \
	git \
	gperf \
	intltool \
	libgdk-pixbuf2.0-dev \
	libffi-dev \
	libgmp-dev \
	libmpc-dev \
	libmpfr-dev \
	libtool \
	libtool-bin \
	libz-dev \
	openssl \
	patch \
	pkg-config \
	p7zip-full \
	ruby \
	scons \
	subversion \
	texinfo \
	unzip \
	wget

# see http://stackoverflow.com/questions/10934683/how-do-i-configure-qt-for-cross-compilation-from-linux-to-windows-target

# Preapre and download cross development environment
RUN mkdir /build
WORKDIR  /build
RUN git clone https://github.com/mxe/mxe.git

# Build cross environment
RUN cd mxe && make qtbase
RUN cd mxe && make qtmultimedia

# TODO: Cleanup all unneeded stuff to make a slim image

# Enhance path
ENV PATH /build/mxe/usr/bin:$PATH

# Add a qmake alias
RUN ln -s /build/mxe/usr/bin/i686-w64-mingw32.static-qmake-qt5 /build/mxe/usr/bin/qmake

# Here the project specific workflow starts.
#
# Now copy the sources. They will become part of the image.
RUN mkdir /src
COPY . /src

# Switch to the source directory
WORKDIR /src

# Now build the project
RUN qmake
RUN make
