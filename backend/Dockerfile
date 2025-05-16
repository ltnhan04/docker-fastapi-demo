#Stage 1: Builder
FROM python:3.12-slim AS builder

WORKDIR /app

COPY pyproject.toml requirements.txt ./
# build the required libs into .whl files into the wheels/ directory -> create local files, do not install immediately
# use it for next step
RUN pip wheel --no-cache-dir --no-deps --wheel-dir wheels -r requirements.txt

COPY src src
# build app into .whl files(def in pyproject.toml) -> whole src code + dependencies in .whl files
RUN pip wheel --no-cache-dir --no-deps --wheel-dir wheels .

# Stage 2: Runner
FROM python:3.12-slim AS runner
COPY --from=builder /app/wheels /wheels
# install .whl -> no needed build, src code -> delete wheels for clean image
RUN pip install --no-cache /wheels/* && rm -rf /wheels

# open port to run FastAPI by Uvicorn
EXPOSE 8000
CMD ["uvicorn", "mysite.main:app", "--host", "0.0.0.0", "--port", "8000"]


