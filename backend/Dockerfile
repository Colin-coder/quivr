# Using a slim version for a smaller base image
FROM python:3.11-slim-bullseye

# Install GEOS library and clean up in one step
RUN apt-get update && apt-get install -y libgeos-dev pandoc binutils && \
    rm -rf /var/lib/apt/lists/* && apt-get clean

WORKDIR /code

# Copy just the requirements first
COPY ./requirements.txt .

RUN pip3 install -i https://pypi.douban.com/simple/ -U pip 
RUN pip3 config set global.index-url https://pypi.douban.com/simple/

# Increase timeout might not be necessary but is retained as in original
RUN pip3 install --no-cache-dir -r requirements.txt --timeout 100

# Copy the rest of the application
COPY . .

CMD ["uvicorn", "main:app", "--reload", "--host", "0.0.0.0", "--port", "5050"]
