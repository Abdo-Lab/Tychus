#########################################################################################################
# Dockerfile to run the Tychus assembly pipeline
#########################################################################################################
FROM ubuntu:16.04
MAINTAINER Chris Dean <cdean11@rams.colostate.edu>


#########################################################################################################
# Install core packages
#########################################################################################################
RUN apt-get update && apt-get install -y \
        aufs-tools \
        automake \
        build-essential \
        wget \
        git \
        libbz2-dev \
        unzip \
	tar \
	zlib1g-dev

#########################################################################################################
# Install Java, Python, and R
#########################################################################################################
RUN apt-get install -y \
	openjdk-8-jre \
	openjdk-8-jdk \
	python \
	r-base-dev

#########################################################################################################
# Install KmerGenie
#########################################################################################################
RUN cd /opt && \
	wget http://kmergenie.bx.psu.edu/kmergenie-1.7016.tar.gz && \
	tar -xvf kmergenie-1.7016.tar.gz && \
	cd kmergenie-1.7016 && \
	make && \
	make install && \
	cd / && \
	rm /opt/kmergenie-1.7016.tar.gz

#########################################################################################################
# Install Trimmomatic
#########################################################################################################
RUN cd /opt && \
        wget http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.36.zip && \
        unzip Trimmomatic-0.36.zip && \
        cd Trimmomatic-0.36 && \
        chmod u+x trimmomatic-0.36.jar && \
        cd / && \
        rm /opt/Trimmomatic-0.36.zip

#########################################################################################################
# Install Prokka and dependencies
#########################################################################################################
RUN cd /opt && \
  	apt-get install -y libdatetime-perl libxml-simple-perl libdigest-md5-perl bioperl && \
  	git clone git://github.com/tseemann/prokka.git && \
  	prokka/bin/prokka --setupdb && \
  	ln -s /opt/prokka/bin/* /usr/bin/ && \
  	cd /

#########################################################################################################
# Install Abyss and dependencies
#########################################################################################################
RUN cd /opt && \
	apt-get install -y libsparsehash-dev libboost1.58-dev && \
	git clone https://github.com/bcgsc/abyss.git && \
	cd abyss && \
	./autogen.sh && \
	./configure --enable-maxk=160 && \
	make && \
	make install && \
	cd / && \
	rm -rf /opt/abyss

#########################################################################################################
# Install IDBA
#########################################################################################################
RUN cd /opt && \
	git clone https://github.com/cdeanj/idba.git && \
	cd idba && \
	./build.sh && \
	ln -s /opt/idba/bin/idba_ud /usr/bin && \
	ln -s /opt/idba/bin/fq2fa /usr/bin && \
	cd /


#########################################################################################################
# Install SPAdes and dependencies
#########################################################################################################
RUN cd /opt && \
	apt-get install -y cmake zlib1g-dev libbz2-dev && \
	wget http://cab.spbu.ru/files/release3.9.1/SPAdes-3.9.1.tar.gz && \
    	tar -xzf SPAdes-3.9.1.tar.gz && \
    	cd SPAdes-3.9.1 && \
	PREFIX=/usr ./spades_compile.sh && \
	cd / && \
	rm -rf /opt/SPAdes-3.9.1.tar.gz /opt/SPAdes-3.9.1

#########################################################################################################
# Install Velvet
#########################################################################################################
RUN cd /opt && \
	git clone https://github.com/dzerbino/velvet.git && \
	cd velvet && \
	make MAXKMERLENGTH=100 && \
	ln -s /opt/velvet/velvetg /opt/velvet/velveth /usr/bin && \
	cd /


#########################################################################################################
# Install CISA and dependencies
#########################################################################################################
RUN cd /opt && \
	apt-get install -y mummer ncbi-blast+ && \
	wget http://sb.nhri.org.tw/CISA/upload/en/2012/9/CISA_20120904-14032356.tar && \
	tar -xvf CISA_20120904-14032356.tar && \
	chmod 755 -R CISA1.2 && \
	ln -s /opt/CISA1.2/*.py /usr/bin && \
	cd / && \
	rm /opt/CISA_20120904-14032356.tar


#########################################################################################################
# Install QUAST and dependencies
#########################################################################################################
RUN cd /opt && \
	apt-get install -y pkg-config libfreetype6-dev libpng-dev python-matplotlib python-setuptools && \
	rm -rf /var/lib/apt/lists/* && \
	wget https://downloads.sourceforge.net/project/quast/quast-4.4.tar.gz && \
	tar -xzf quast-4.4.tar.gz && \
	cd quast-4.4 && \
	./setup.py install && \
	cd / && \
	rm -rf /opt/quast-4.4 /opt/quast-4.4.tar.gz

#########################################################################################################
# Install Velvet Optimiser
#########################################################################################################
RUN cd /opt && \
	git clone https://github.com/tseemann/VelvetOptimiser.git && \
	ln -s /opt/VelvetOptimiser/VelvetOptimiser.pl /usr/bin && \
	ln -s /opt/VelvetOptimiser/VelvetOpt /usr/bin && \
	cd /

#########################################################################################################
# Install Tbl2san
#########################################################################################################
RUN wget -N ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools/converters/by_program/tbl2asn/linux64.tbl2asn.gz && \
	gunzip linux64.tbl2asn.gz && \
	chmod +x linux64.tbl2asn && \
	mv linux64.tbl2asn /opt/prokka/binaries/linux/tbl2asn && \
	cd /
	
	

RUN mkdir /SCRATCH
WORKDIR /SCRATCH
CMD ["bash"]	
