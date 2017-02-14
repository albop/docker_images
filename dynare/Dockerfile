FROM andrewosh/binder-base

# MAINTAINER Andrew Osheroff <andrewosh@gmail.com>


USER root
RUN apt-get update && apt-get clean
RUN apt-get install -y build-essential gfortran autoconf automake flex bison
RUN apt-get install -y libboost-dev texinfo

USER main
RUN git clone https://github.com/DynareTeam/dynare.git
RUN cd dynare && git fetch && git checkout json
RUN cd dynare && autoreconf -si 
RUN cd dynare && ./configure --disable-octave --disable-octave LDFLAGS="-static"
RUN cd dynare && make
