FROM ubuntu
MAINTAINER Pablo Winant

RUN apt-get update
RUN apt-get install -y wget

RUN useradd -ms /bin/bash dynosaur

WORKDIR /home/dynosaur/

USER dynosaur


#RUN wget https://repo.continuum.io/archive/Anaconda3-4.3.0-Linux-x86_64.sh && bash ./Anaconda3-4.3.0-Linux-x86_64.sha
RUN wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
USER root
RUN apt-get install bzip2
USER dynosaur
RUN bash Miniconda3-latest-Linux-x86_64.sh -b -p /home/dynosaur/.opt/miniconda
RUN /home/dynosaur/.opt/miniconda/bin/conda install -y -c conda-forge jupyterlab
ENV PATH /home/dynosaur/.opt/miniconda/bin:$PATH
EXPOSE 8888

#USER root
#RUN wget https://github.com/Yelp/dumb-init/releases/download/v1.1.1/dumb-init_1.1.1_amd64.deb && dpkg -i dumb-init_*.deb
#ENTRYPOINT ["dumb-init", "--"]
#USER dynosaur

ENV NBUSER=dynosaur

##################
###### Julia #####
##################
USER root
RUN touch /home/$NBUSER/.curlrc
RUN chown $NBUSER /home/$NBUSER/.curlrc

USER $NBUSER
RUN echo 'cacert=/etc/ssl/certs/ca-certificates.crt' > $HOME/.curlrc
RUN wget https://julialang.s3.amazonaws.com/bin/linux/x64/0.5/julia-0.5.0-linux-x86_64.tar.gz
RUN mkdir $HOME/julia
RUN tar xvf julia-0.5.0-linux-x86_64.tar.gz -C $HOME/julia --strip-components=1
ENV PATH /home/dynosaur/julia/bin:$PATH


USER root
RUN apt-get install --no-install-recommends -y git libzmq5


USER $NBUSER
# Install IJulia kernel
RUN julia -e 'ENV["JUPYTER"]="/home/dynosaur/.opt/miniconda/bin/jupyter";Pkg.add("IJulia")'
RUN julia -e 'Pkg.add("PyPlot")'


USER root
RUN apt-get install --no-install-recommends -y build-essential make gfortran
USER $NBUSER

RUN julia -e 'Pkg.add("Gadfly")'
RUN julia -e 'Pkg.add("SymEngine")'
RUN julia -e 'Pkg.clone("https://github.com/EconForge/Dolang.git")'
RUN julia -e 'Pkg.clone("https://github.com/EconForge/Dyno.git")'
RUN julia -e 'Pkg.build("Dyno") '
RUN julia -e 'using Dyno;using IJulia;using SymEngine'

USER root
RUN apt-get install --no-install-recommends -y hdf5-tools unzip
USER $NBUSER
RUN julia -e 'Pkg.clone("https://github.com/EconForge/Dolo.jl.git")'
RUN julia -e 'cd(Pkg.dir("YAML"));run(`git checkout tags`)'
RUN julia -e 'Pkg.clone("https://github.com/EconForge/splines.jl.git")'
RUN julia -e 'using Dolo;using YAML'
USER root
RUN apt-get install --no-install-recommends -y nano
USER $NBUSER


