# Sunspot::NullResult [![Build Status](https://travis-ci.org/Absolventa/sunspot_null-result.svg?branch=master)](https://travis-ci.org/Absolventa/sunspot_null-result)

Interface taken from `Sunspot::Rails::StubSessionProxy::PaginatesCollection` of
[sunspot-rails](https://github.com/sunspot/sunspot/tree/master/sunspot_rails).

It adds the option to inject records to mimic actual search results (and yes,
thus making fun of the "Null" gem name).

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'sunspot-null_result'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install sunspot-null_result

## Usage

RSpec case:

```ruby
RSpec.describe 'stubbed Solr result' do
  let(:search) { Sunspot::NullResult.new(records) }
  before { allow(MyModel).to receive(:search).and_return(search) }

  context 'with no records' do
    let(:records) { [] }
    # …
  end

  context 'with some records' do
    let(:records) { [MyModel.new, MyModel.new] }
    # …
  end
end
```

Rescue case for unavailable Solr server, e.g. Websolr having issues.

```ruby
class ApplicationController < ActionController::Base

  rescue_from RSolr::Error::Http do |exception|
    # Handle exception so that a complete server shutdown
    # doesn't go unnoticed. Your app may still work using:

    failover_records = MyModel.last(10)
    Sunspot::NullResult.new(failover_records)
  end

  # …
end
```

## Changelog

### HEAD (not yet released)

### v0.1.0
* Initial working release


## License

The gem is available as free and open source software under the terms of the
[MIT License](http://opensource.org/licenses/MIT).

