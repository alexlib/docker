# Minimal python container
# based on [progrium/busybox](https://github.com/progrium/busybox) and [Anaconda Python Distribution](https://store.continuum.io/cshop/anaconda/)

FROM progrium/busybox
MAINTAINER Alex Liberzon
RUN opkg-install bash bzip2 zlib git check
ADD conda_install.sh /root/conda_install.sh
RUN ["bash", "/root/conda_install.sh"]
ENV PATH /root/miniconda/bin:$PATH
RUN conda update -y conda
# Python packages from conda
RUN conda install -y \
    scikit-image \
    flask \
    pillow \
    chaco  \
    cython \
    wxpython \
    cmake
    

RUN git clone git://github.com/OpenPTV/openptv.git
RUN mkdir openptv/liboptv/build
RUN cd openptv/liboptv/build
RUN cmake ../
RUN make
RUN sudo make install
RUN make verify

RUN cd ~
RUN git clone git://github.com/OpenPTV/openptv-python.git
RUN cd openptv-python/pyptv_gui
RUN python setup.py build_ext --inplace
RUN python setup.py install

RUN cd ~
RUN git clone https://bitbucket.org/turbulencelabtau/ptv_test_folder.git
RUN mkdir ptv_test_folder/res

RUN LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH python ~/openptv-python/pyptv_gui/pyptv_gui.py ~/ptv_test_folder/


