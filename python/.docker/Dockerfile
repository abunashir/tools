ARG APP_PATH="/app"
ARG APP_NAME=insights
ARG BASE_IMAGE=python:3.10-bullseye
ARG BASE_IMAGE_SLIM=python:3.10-slim-bullseye

#
# Stage: staging
#
FROM $BASE_IMAGE as staging
ARG APP_PATH

ENV \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONFAULTHANDLER=1

ENV \
    POETRY_HOME="/opt/poetry" \
    POETRY_VIRTUALENVS_IN_PROJECT=true \
    POETRY_NO_INTERACTION=1

# install poetry
RUN curl -sSL https://install.python-poetry.org | python -
ENV PATH="$POETRY_HOME/bin:$PATH"

# Import our project files
WORKDIR $APP_PATH
COPY ./poetry.lock ./pyproject.toml ./

#
# Stage: Development
#
FROM staging as development
ARG APP_PATH
WORKDIR $APP_PATH

EXPOSE 8080
ENTRYPOINT ["poetry", "run"]

#
# Stage: build
#
FROM staging as build
ARG APP_PATH

WORKDIR $APP_PATH
COPY . .

RUN poetry build --format wheel
RUN poetry export --format requirements.txt --output constraints.txt --without-hashes

#
# Stage: production
#
FROM $BASE_IMAGE_SLIM as production
ARG APP_PATH

ENV \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONFAULTHANDLER=1

ENV \
    PIP_NO_CACHE_DIR=off \
    PIP_DISABLE_PIP_VERSION_CHECK=on \
    PIP_DEFAULT_TIMEOUT=100

WORKDIR $APP_PATH
COPY --from=build $APP_PATH/dist/*.whl ./
COPY --from=build $APP_PATH/constraints.txt ./

RUN pip install ./$APP_NAME*.whl -r constraints.txt --ignore-installed
RUN rm *.whl constraints.txt

# Copy over any app data
COPY bin/serve-production conf/gunicorn.py ./
COPY data ./data

EXPOSE 8080
RUN chmod +x ./serve-production
CMD ["./serve-production"]
