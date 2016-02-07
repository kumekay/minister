Minister
========

Minimalistic finite state machine for Ruby with [Redis][redis] data storage

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'minister'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install minister

## Usage

Create new state machine object:

```ruby
require 'minister'

# Create new machine
fsm = Minister.new

# Describe states, initial state first.
fsm.states = [:green, :yellow, :red, :blinking_yellow]

# Describe events and transactions
fsm.event(:go, from: [:red, :blinking_yellow], to: :green)
fsm.event(:warn, from: [:green, :blinking_yellow], to: :yellow)
fsm.event(:stop, from: [:yellow, :blinking_yellow], to: :yellow)
fsm.event(:maintenance, from: fsm.states - [:blinking_yellow], to: :blinking_yellow)

# Current state
fsm.state # => :green
fsm.green? # => true
fsm.yellow? # => false

# Trigger event
fsm.warn!

fsm.state # => :yellow
fsm.green? # => false
fsm.yellow? # => true
```

Minister requires [redis-rb][redis.rb] gem and works with ruby 2.0+

## Development

After checking out the repo, run `bin/setup` to install dependencies. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/pinya/minister. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

[redis]: http://redis.io
[redis.rb]: https://github.com/redis/redis-rb
