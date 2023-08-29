# Using a slim version for a smaller base image
FROM python:3.11-slim-bullseye

# Install GEOS library, Rust, and other dependencies, then clean up
RUN apt-get update && apt-get install -y \
    libgeos-dev \
    pandoc \
    binutils \
    curl \
    build-essential && \
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y && \
    rm -rf /var/lib/apt/lists/* && apt-get clean

# Add Rust binaries to the PATH
ENV PATH="/root/.cargo/bin:${PATH}"

WORKDIR /code

# Copy just the requirements first
COPY ./requirements.txt .

RUN pip3 install -i https://pypi.douban.com/simple/ -U pip 
RUN pip3 config set global.index-url https://pypi.douban.com/simple/

# Increase timeout might not be necessary but is retained as in original
RUN pip3 install --no-cache-dir -r requirements.txt --timeout 200

# Copy the rest of the application
COPY . .

CMD ["uvicorn", "main:app", "--reload", "--host", "0.0.0.0", "--port", "5050"]
