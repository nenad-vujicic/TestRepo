# TestRepo

Used for testing GitHub workflows and Ruby on Rails

1. Starts a new container, based on Ubuntu 22.04, and generates new Ruby on Rails application by executing following steps:

  a) Goes into non-interactive mode (export DEBIAN_FRONTEND=noninteractive) because tzdata package requires user interaction
  b) Updates the packages to ensure it's up to date (apt-get update)
  c) Installs packages, without recommended packages and with automatic YES answer
  d) Installs the Ruby On Rails framework (gem install rails)
  e) Creates new Ruby On Rails application in current directory with overwritting existing files and skipping dependencies (rails new . --force --no-deps)
  f) New application is generated at LOCAL_PATH on host machine
  
docker run --rm -v LOCAL_PATH:/app -w /app ubuntu:22.04 sh -c "export DEBIAN_FRONTEND=noninteractive && apt-get update && apt-get install --no-install-recommends -y build-essential curl default-jre-headless file git-core gpg-agent libarchive-dev libffi-dev libgd-dev libpq-dev libsasl2-dev libvips-dev libxml2-dev libxslt1-dev libyaml-dev locales postgresql-client ruby ruby-dev ruby-bundler software-properties-common tzdata unzip && gem install rails && rails new . --force --no-deps"

2. Creates new docker image for running generated Ruby On Rails application by executing following steps:

  a) Update Dockerfile by inserting (before "RUN useradd rails"):
      # Set Rails credentials environment variables
      ARG SECRET_KEY_BASE
      ENV SECRET_KEY_BASE=$SECRET_KEY_BASE
  d) Generate docker image using secret key (docker build --build-arg SECRET_KEY_BASE=secret_key -t ror .)

docker build -t ror .
docker run --rm ror ./bin/rails secret
docker build --build-arg SECRET_KEY_BASE=secret_key -t ror .

3. Run created image and dockerized image by executing: docker run --rm -it -p 3000:3000 ror
