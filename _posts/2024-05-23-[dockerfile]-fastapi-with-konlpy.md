---
title: "[Dockerfile] FastAPI + KoNLPy"
description: Dockerfile 공유!
date: 2024-05-23T07:23:35.664Z
preview: ""
tags:
    - Docker
    - FastAPI
    - KoNLPy
categories:
    - CI/CD
    - Docker
type: default
---

교내 프로젝트를 진행하면서 FastAPI와 KoNLPy를 도커로 올려야 할 일이 있었다. <br>
그러나 구글링을 해봐도 속 시원하게 대답해주거나 동작하는 글을 찾기 어려워서, 내 결과물을 공유한다. <br>
> ChatGPT의 도움을 받아서 작성했다!

```Dockerfile
# Use a lightweight base image
FROM python:3.12-slim

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Install system dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends default-jdk \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Set JAVA_HOME environment variable
ENV JAVA_HOME /usr/lib/jvm/default-java

# Set work directory
WORKDIR /app

# Install Python dependencies
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt

# Copy the application code
COPY . /app/

# Expose port (if needed)
EXPOSE 8000

# Command to run the application
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```