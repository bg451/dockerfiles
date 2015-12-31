FROM golang:1.5.2

# see https://golang.org/doc/install/source#environment
# see also http://build.golang.org/
# and canonically, see defs_OS_ARCH.h files in src/runtime
#   https://go.googlesource.com/go/+/go1.4.1/src/runtime/
ENV GOLANG_CROSSPLATFORMS \
	darwin/amd64 \
	linux/amd64 \
	windows/amd64

# TODO gcc: error: unrecognized command line option '-marm'
#	android/arm \

# ls src/runtime/defs_*_*.h | sed -r 's!^.*/defs_([^_]+)_([^_]+)[.]h$!\1/\2!'

# (set an explicit GOARM of 5 for maximum ARM compatibility)
ENV GOARM 5

# We need go1.4 to actually build everything
RUN curl -fsSL "https://storage.googleapis.com/golang/go1.4.2.linux-amd64.tar.gz" -o golang.tar.gz \
	&& tar -C /root -xzf golang.tar.gz \
  && mv /root/go /root/go1.4 \
	&& rm golang.tar.gz

RUN cd /usr/local/go/src \
	&& set -ex \
	&& for platform in $GOLANG_CROSSPLATFORMS; do \
		GOOS=${platform%/*} \
		GOARCH=${platform##*/} \
		./make.bash --no-clean 2>&1; \
	done
