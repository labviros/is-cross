FROM dockcross/linux-armv6

## Change Accordingly
#ENV GIT_URL https://github.com
ENV GIT_URL http://192.168.1.101

ENV N_CORES 3 
##

#RUN dpkg --add-architecture armhf && apt update

RUN wget https://sourceforge.net/projects/boost/files/boost/1.63.0/boost_1_63_0.tar.gz \
 && tar -xvf boost_1_63_0.tar.gz \
 && cd boost_1_63_0 \
 && ./bootstrap.sh \
 && echo 'using gcc : arm : arm-linux-gnueabihf-g++ ; ' | cat - project-config.jam > tmp && mv tmp project-config.jam \
 && ./b2 install cxxflags=-std=gnu++0x --with-timer --with-chrono --with-system --with-fiber \
    --with-program_options --with-filesystem --with-thread --with-date_time link=shared --prefix=/usr/arm-linux-gnueabihf -j $N_CORES \
 && cd /work

# Install 1.0.2 since 1.1 is not compatible with older versions and breaks rabbitmq-c
RUN git clone $GIT_URL/labviros/openssl.git && cd openssl && git checkout OpenSSL_1_0_2-stable \
 && ./Configure linux-generic32 --prefix=/usr/arm-linux-gnueabihf \
 && make -j $N_CORES \
 && make install_sw \
 && cd /work

RUN git clone $GIT_URL/labviros/rabbitmq-c && cd rabbitmq-c \
 && mkdir build && cd build \
 && cmake .. -DCMAKE_INSTALL_PREFIX=/usr/arm-linux-gnueabihf -DBUILD_EXAMPLES=OFF -DBUILD_TESTS=OFF -GNinja \
 && ninja install -j $N_CORES \
 && cd /work 
 
RUN git clone $GIT_URL/labviros/SimpleAmqpClient && cd SimpleAmqpClient \
 && mkdir build && cd build \
 && cmake .. -DCMAKE_INSTALL_PREFIX=/usr/arm-linux-gnueabihf -GNinja \
 && ninja install -j $N_CORES \
 && cd /work 

# We have to build protoc for our arch first then build protobuf for arm 
RUN git clone $GIT_URL/labviros/protobuf && cd protobuf \
 && apt install -y unzip \
 && ./autogen.sh \ 
 && ./configure CC=gcc CXX=g++ \
 && make install -j $N_CORES \
 && make clean \
 && ./configure --host=arm --prefix=/usr/arm-linux-gnueabihf --with-protoc=/usr/local/bin/protoc \
 && make install -j $N_CORES \
 && cd /work 

RUN git clone $GIT_URL/labviros/spdlog && cd spdlog \
 && mkdir build && cd build \
 && cmake .. -DCMAKE_INSTALL_PREFIX=/usr/arm-linux-gnueabihf -GNinja \
 && ninja install -j $N_CORES \
 && cd /work 

RUN git clone $GIT_URL/labviros/opentracing-cpp && cd opentracing-cpp \
 && mkdir build && cd build \
 && cmake .. -DCMAKE_INSTALL_PREFIX=/usr/arm-linux-gnueabihf -GNinja \
 && ninja install -j $N_CORES \
 && cd /work 

RUN git clone $GIT_URL/labviros/curl && cd curl \
 && mkdir build && cd build \
 && cmake .. -DCMAKE_INSTALL_PREFIX=/usr/arm-linux-gnueabihf -GNinja \
 && ninja install -j $N_CORES \
 && cd /work 

RUN git clone $GIT_URL/labviros/zipkin-cpp-opentracing && cd zipkin-cpp-opentracing \
 && mkdir build && cd build \
 && cmake .. -DCMAKE_INSTALL_PREFIX=/usr/arm-linux-gnueabihf -GNinja \
 && ninja install -j $N_CORES \
 && cd /work 

RUN git clone $GIT_URL/labviros/prometheus-cpp && cd prometheus-cpp \
 && git submodule update --init \ 
 && mkdir build && cd build \
 && head -n -1 ../CMakeLists.txt > tmp && mv tmp ../CMakeLists.txt \
 && cmake .. -DCMAKE_INSTALL_PREFIX=/usr/arm-linux-gnueabihf -GNinja \
 && ninja install -j $N_CORES \
 && cd /work 

RUN git clone $GIT_URL/labviros/is-cpp2 && cd is-cpp2 \
 && mkdir build && cd build \
 && cmake .. -DCMAKE_INSTALL_PREFIX=/usr/arm-linux-gnueabihf -GNinja \
 && ninja install -j $N_CORES \
 && cd /work

RUN git clone $GIT_URL/labviros/is-msgs && cd is-msgs \
 && mkdir build && cd build \
 && cmake .. -DCMAKE_INSTALL_PREFIX=/usr/arm-linux-gnueabihf -GNinja \
 && ninja install -j $N_CORES \
 && cd /work

RUN rm -rf /work/*
