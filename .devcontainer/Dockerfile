FROM mcr.microsoft.com/devcontainers/universal:2.5-linux as rust_build
RUN apt-get update && apt-get install -y cargo
WORKDIR /build
COPY src/rust/rust-demultiplex /build/rust-demultiplex
RUN cd /build/rust-demultiplex && cargo build --release && cp target/release/rust-demultiplex /usr/bin/rust-demultiplex
COPY src/rust/rust-umi /build/rust-umi
RUN cd /build/rust-umi && cargo build --release && cp target/release/rust-umi /usr/bin/rust-umi
FROM mcr.microsoft.com/devcontainers/universal:2.5-linux
COPY --from=rust_build /usr/bin/rust-demultiplex /usr/bin/rust-demultiplex
COPY --from=rust_build /usr/bin/rust-umi /usr/bin/rust-umi
COPY src/python/spades_assembler.py /usr/bin/spades_assembler.py
RUN apt-get update && apt-get install -y pigz
RUN wget https://github.com/usadellab/Trimmomatic/files/5854859/Trimmomatic-0.39.zip && unzip Trimmomatic-0.39.zip && rm -r -f Trimmomatic-0.39.zip
RUN wget https://github.com/ablab/spades/releases/download/v3.15.5/SPAdes-3.15.5-Linux.tar.gz && tar xvzf SPAdes-3.15.5-Linux.tar.gz && rm SPAdes-3.15.5-Linux.tar.gz && cp -r SPAdes-3.15.5-Linux/bin/* /usr/bin && cp -r SPAdes-3.15.5-Linux/share/* /usr/share
RUN wget https://github.com/bwa-mem2/bwa-mem2/releases/download/v2.2.1/bwa-mem2-2.2.1_x64-linux.tar.bz2 && tar xvjf bwa-mem2-2.2.1_x64-linux.tar.bz2 && cp bwa-mem2-2.2.1_x64-linux/bwa* /usr/bin && rm -r -f bwa-mem2-2.2.1_x64-linux.tar.bz2 bwa-mem2-2.2.1_x64-linux