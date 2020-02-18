FROM nvidia/cuda:10.2-runtime

USER root

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

RUN apt-get update --fix-missing && \
    apt-get install -y wget software-properties-common curl

RUN wget --quiet https://repo.anaconda.com/archive/Anaconda2-2019.10-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc


RUN conda create -n pytorch python=3.6 anaconda jupyter pytorch=1.3 pytorch-gpu

RUN apt-get install -y tmux vim htop tmux zsh git zip unzip

RUN /bin/sh -c "conda update conda && conda init zsh"
RUN /bin/zsh -c "source /root/.zshrc && conda activate pytorch"
RUN echo "conda activate pytorch" >> ~/.bashrc && echo "conda activate pytorch" >> ~/.zshrc
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
RUN git clone --depth=1 https://github.com/amix/vimrc.git ~/.vim_runtime && \
   sh ~/.vim_runtime/install_awesome_vimrc.sh
RUN sed -i 's/robbyrussell/junkfood/g' /root/.zshrc

EXPOSE 7777

ENTRYPOINT ["/bin/zsh", "-c"]
CMD ["export SHELL=/bin/zsh && conda init zsh && source /root/.zshrc && conda activate pytorch && jupyter notebook --no-browser --port=7777 --ip=0.0.0.0 --allow-root --config=/root/.jupyter/jupyter_notebook_config.json"]
