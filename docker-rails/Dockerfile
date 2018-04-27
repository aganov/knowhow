FROM ruby:2.4

RUN echo 'deb http://apt.postgresql.org/pub/repos/apt/ jessie-pgdg main' | tee /etc/apt/sources.list.d/pgdg.list > /dev/null
RUN wget --no-check-certificate -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O- | apt-key add -
RUN apt-get update && apt-get install -y libpq-dev postgresql-client-10

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash
RUN apt-get install -y nodejs

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install -y yarn

RUN mkdir /app
WORKDIR /app

COPY Gemfile Gemfile.lock ./
RUN bundle install --binstubs

COPY package.json yarn.lock ./
RUN yarn install

COPY . .

CMD ["rails", "s"]