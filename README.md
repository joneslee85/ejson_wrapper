# EjsonWrapper

Wraps the EJSON go program to safely execute it and parse the resulting JSON.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'ejson_wrapper'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install ejson_wrapper

## Usage

### Decrypting EJSON files

From Ruby:

```
# Private key is in /opt/ejson/keys
EJSONWrapper.decrypt('myfile.ejson')
=> { :my_api_key => 'secret' }

# Private key is in /alternate/key/dir
EJSONWrapper.decrypt('myfile.ejson', key_dir: 'alternate/key/dir')
=> { :my_api_key => 'secret' }

# Private key is in memory
EJSONWrapper.decrypt('myfile.ejson', private_key: 'be8597abaa68bbfa23193624b1ed5e2cd6b9a8015e722138b23ecd3c90239b2d')
=> { :my_api_key => 'secret' }

# Private key is stored inside the ejson file itself as _private_key_enc (encrypted with KMS & Base64 encoded)
EJSONWrapper.decrypt('myfile.ejson', use_kms: true, region: 'ap-southeast-2')
=> { :my_api_key => 'secret' }
```

Command line:

```
# decrypt all
$ ejson_wrapper decrypt --file file.ejson --region us-east-1
{
  "datadog_api_token": "[datadog_api_token]"
}

# decrypt & extract a specific secret
$ ejson_wrapper decrypt --file file.ejson --region us-east-1 --secret datadog_api_token
[datadog_api_token]
```

### Generating EJSON files

```
$ ejson_wrapper generate --region ap-southeast-2 --kms-key-id [key_id] --file file.ejson
Generated EJSON file file.ejson

$ cat file.ejson
{
  "_public_key": "[public_key]",
  "_private_key_enc":"[encrypted_private_key]"
}
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/envato/ejson_wrapper.
