FROM ruby:2.3

WORKDIR /myapp

RUN apt-get update && apt-get install -qq -y --no-install-recommends apt-transport-https build-essential libpq-dev

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update && apt-get install -qq -y --no-install-recommends nodejs	yarn
	

COPY Gemfile Gemfile.lock package.json ./ 

RUN bundle check || \
    bundle install --clean --jobs 5 --quiet --retry 5

RUN yarn install

COPY . .

RUN rake RAILS_ENV=development assets:precompile

EXPOSE 3000

CMD ["bundle", "exec", "puma", "-C", "config/puma.rb"]