VERSION 0.7

ARG --global XFSPROGS_SRC_RPM=https://kojipkgs.fedoraproject.org//packages/xfsprogs/6.5.0/3.fc40/src/xfsprogs-6.5.0-3.fc40.src.rpm

rpmbuild:
    ARG F_BUILD_ENV=40

    FROM fedora:${F_BUILD_ENV}

    RUN dnf install -y rpm-build joe rpmdevtools ncurses-devel pesign grubby python3-dnf-plugins-core python-rpm-macros && \
      rpmdev-setuptree

    WORKDIR /root/rpmbuild

    RUN curl -sSLo SRPMS/xfsprogs.src.rpm $XFSPROGS_SRC_RPM && \
          rpm -ivh SRPMS/xfsprogs.src.rpm

    COPY SOURCES/* SOURCES/
    COPY SPECS/* SPECS/

    ARG SMP_MFLAGS='-j16'

    RUN echo "%_smp_mflags ${SMP_MFLAGS}" >>~/.rpmmacros && \
      dnf builddep -y SPECS/xfsprogs.spec && \
      rpmbuild -ba SPECS/xfsprogs.spec

    SAVE ARTIFACT --keep-ts /root/rpmbuild/RPMS/* AS LOCAL ./RPMS/

build:
    BUILD +rpmbuild
