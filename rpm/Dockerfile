# Build el7 rpm for flameshot
#
# podman build -t flameshot:el7 -f rpm/Dockerfile --build-arg "VERSION=0.6.0" .
# podman cp $(podman create flameshot:el7):/root/rpmbuild/RPMS/x86_64/. rpm/


FROM centos:7
ARG VERSION=0.6.0

COPY . /v${VERSION}

# Install build requirements
RUN set -x && \
    yum install -y centos-release-scl-rh && \
    yum install -y \
        git \
        make \
        devtoolset-3-gcc-c++ \
        qt5-qttools-devel \
        qt5-qtsvg-devel \
        rpmdevtools && \
    yum clean all && \
    update-alternatives --install /usr/bin/gcc gcc-4.9 /opt/rh/devtoolset-3/root/usr/bin/gcc 10 && \
    update-alternatives --install /usr/bin/g++ g++-4.9 /opt/rh/devtoolset-3/root/usr/bin/g++ 10

# Create rpmbuild directory
RUN set -x && \
    rpmdev-setuptree && \
    tar zcvf /root/rpmbuild/SOURCES/v${VERSION}.tar.gz /v${VERSION} && \
    sed -e '/BuildRequires: gcc-c++/d' /v${VERSION}/rpm/flameshot.spec > /root/rpmbuild/SPECS/flameshot.spec

# Build srpm and rpm
RUN rpmbuild -vv -ba /root/rpmbuild/SPECS/flameshot.spec
