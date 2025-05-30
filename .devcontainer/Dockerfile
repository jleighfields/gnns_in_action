# Based on NGC PyG 24.09 image:
# https://docs.nvidia.com/deeplearning/frameworks/pyg-release-notes/rel-24-09.html#rel-24-09

FROM pytorch/pytorch:2.7.0-cuda12.8-cudnn9-devel
#FROM nvcr.io/nvidia/cuda-dl-base:24.09-cuda12.6-devel-ubuntu22.04
#FROM nvidia/cuda:12.8.1-cudnn-devel-ubuntu22.04

# install pip
RUN apt-get update && apt-get install -y python3-pip
RUN apt-get -y install git

# install uv
# https://docs.astral.sh/uv/guides/integration/docker/#installing-uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# install PyTorch - latest stable version
RUN uv pip install --system torch torchvision torchaudio

# install graphviz - latest stable version
RUN apt-get install -y graphviz graphviz-dev
RUN uv pip install --system pygraphviz

# install python packages with NGC PyG 24.09 image versions
RUN uv pip install --system torch_geometric==2.6.0
RUN uv pip install --system triton==3.0.0 numba==0.59.0 requests==2.32.3 opencv-python==4.7.0.72 scipy==1.14.0 jupyterlab==4.2.5

# install cugraph
RUN uv pip install --system cugraph-cu12 cugraph-pyg-cu12 --extra-index-url=https://pypi.nvidia.com

# it may take a moment to install these, keep the faith
RUN uv pip install --system torch_scatter torch_sparse torch_cluster torch_spline_conv --no-build-isolation
RUN uv pip install --system scikit-learn darts \
polars duckdb pandas pyarrow seaborn plotly \
python-dotenv xgboost \
jupyterlab jupyterlab-git ipywidgets \
pytest pylint rsconnect-python

RUN uv pip install --system networkx matplotlib node2vec umap-learn

RUN echo "Finished setting up..."

EXPOSE 8888-8899 8501

# set jupyter lab configurations
# https://stackoverflow.com/questions/61973485/how-do-i-launch-jupyter-lab-with-the-dark-theme-enabled-by-default
# https://jupyterlab.readthedocs.io/en/stable/user/directories.html#settings
COPY overrides.json /opt/conda/share/jupyter/lab/settings/

ENTRYPOINT ["jupyter", "lab","--ip=0.0.0.0","--port=8899","--allow-root","--ServerApp.token=gnn"]
