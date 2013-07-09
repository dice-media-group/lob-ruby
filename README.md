# lob-ruby

Ruby wrapper for the [Lob.com](http://lob.com) API. This gem gives you an ActiveRecord-style syntax to use the Lob.com API.

Supports Ruby 1.9.3 and greater.

## Installation

Add this line to your application's `Gemfile`:

    gem 'lob'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install lob

## Usage

The library uses an ActiveRecord-style interface. You'll feel right at home.
You'll need a Lob.com API key. It's free and you can get yours [here](https://www.lob.com/account).

For optional parameters and other details, refer the docs here - <https://lob.com/docs>

### Initialization and configuration

```ruby
# To initialize a Lob object
@lob = Lob(api_key: "your-api-key")


# Alternatively, to set the API key for all calls in the future
Lob.api_key = "you-api-key"
@lob = Lob()   # don't forget the paranthesis!
```

#### Or if you want some detailed configuration

```ruby
Lob.configure do |config|
  config.api_key = "your-api-key"  # get your own at http://lob.com :)
  config.api_version = "v1"        # default version
  config.protocol = "https"        # default protocol
  config.api_host = "api.lob.com"  # ofcourse it's Lob
end
```

### Addresses

#### To create an address

```ruby
# name, address, city, state, country and zip are required parameters
@lob.addresses.create(
  name: "John Doe",
  address_line1: "104, Printing Boulevard",
  city: "Boston",
  state: "MA",
  country: "USA"
  zip: 12345
)

# You can also pass address_line2
@lob.addresses.create(
  name: "John Doe",
  email: "test@test.com",  # see you can also pass other optional parameters?
  address_line1: "104, Printing Boulevard",
  address_line2: "Sunset Town",
  city: "Boston",
  state: "MA",
  country: "USA"
  zip: 12345
)
```

#### List addresses

```ruby
# returns an array of addresses
@lob.addresses.list

#you can also pass count and offset
@lob.addresses.list(count: 10, offset: 3)
```

#### Find a specific address

```ruby
# returns the address with the corresponding ID
@lob.addresses.find("some-address-id")
```

#### Deletes a specific address

```ruby
# deletes the address with the corresponding ID
@lob.addresses.destroy("some-address-id")
```

### Address verification

```ruby
# verifies and returns an address with more details
@lob.addresses.verify(
      address_line1: "220 WILLIAM T MORRISSEY BLVD",
      city:    "Boston",
      state:   "MA",
      zip:     02125
  )
```

### Jobs

#### Create jobs

```ruby
# name, to-address and object are the arguments
# to-address can be specified as an address-id
@lob.jobs.create("New Cool Posters", "to-address-id", "object-id")

# to-address can also be specified as address params to create new address
@lob.jobs.create(
  "New Cool Posters",
  {name: "ToAddress", address_line1: "120, 6th Ave", city: "Boston", country: "USA", zip: 12345},
  "object-id"
)

# You can also pass new object params for the object
# and other options like packaging_id an setting_id
@lob.jobs.create(
  "New Cool Posters",
  "to-address-id",
  "object-id",
  {
    name: "Your fantistic object",
    file: "http://test.com/file.pdf",
    setting_id: "some-setting-id"
  }
)
```

#### List jobs

```ruby
# returns an array of jobs
@lob.jobs.list

#you can also pass count and offset
@lob.jobs.list(count: 10, offset: 3)
```

#### Find a specific job

```ruby
# returns the job with the corresponding ID
@lob.jobs.find("some-job-id")
```

### Objects

#### Create objects

```ruby
# You can create by passing the name, file url and setting ID
@lob.objects.create(
  "Your fantistic object",
  "http://test.com/file.pdf",
  "some-setting-id"
)

# You can also pass the quantity as an option
# Or pass a file for upload instead of a url
@lob.objects.create(
  "Your fantistic object",
  File.new("/path/to/file.pdf"),
  "some-setting-id",
  quantity: 12
)
```

#### List objects

```ruby
# returns an array of objects
@lob.objects.list

#you can also pass count and offset
@lob.objects.list(count: 10, offset: 3)
```

#### Find a specific object

```ruby
# returns the object with the corresponding ID
@lob.objects.find("some-object-id")
```

#### Delete a specific object

```ruby
# deletes the object with the corresponding ID
@lob.objects.destroy("some-object-id")
```

### Packagings

#### List packagings

```ruby
# returns a list of packagings
@lob.packagings.list
```

### Postcards

#### Creating post cards

You'll have to specify either the `message` option or the `back` option.

```ruby
# accepts the name, address-id to send to and options
@lob.postcards.create(
  "John Joe",
  "to-address-id",
  message: front: File.read("/path/to/file.pdf")
)

# create using address params, front, back and from address
@lob.postcards.create(
  "John Joe",
  {name: "ToAddress", address_line1: "120, 6th Ave", city: "Boston", country: "USA", zip: 12345},
  message: "Hey buddy. Waiting to hear your stories",
  front: "http://test.com/file.pdf",
  back: File.read("/path/to/file.pdf"),
  from: {name: "FromAddress", address_line1: "120, 6th Ave", city: "Boston", country: "USA", zip: 12345},
)
```

#### List postcards

```ruby
@lob.postcards.list

#you can also pass count and offset
@lob.postcards.list(count: 10, offset: 3)
```

#### Find a postcard

```ruby
@lob.postcards.find "post-card-id"
```

### Services

#### List services

```ruby
# returns a list of services
@lob.services.list
```

### Settings

#### List settings

```ruby
# returns a list of settings
@lob.settings.list
```

## Developing

Make sure you have Ruby 2.0 installed. Copy and paste the following commands in your projects directory.

    git clone https://github.com/lob/lob-ruby.git
    cd lob
    bundle install

You are powered up and ready to roll ~!

## Running the test-suite

Tests are written using MiniTest, a testing library that comes with Ruby stdlib. The remote responses are tested using [vcr](https://github.com/vcr/vcr).

You'll need to pass in your Lob.com API as the environment variable `LOB_API_KEY`, to run the tests. Be sure to use your Test API key, and not the Live one.

Here's how you can run the tests:

    LOB_API_KEY=your_test_api_key bundle exec rake test


## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Make sure the tests pass and add tests if required
6. Create new Pull Request

## Credits

* [Akash Manohar J](http://github.com/HashNuke)

Copyright &copy; 2013 Lob.com

Released under the MIT License, which can be found in the repository in `LICENSE.txt`.
