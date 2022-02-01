# Enummer

[![Gem](https://img.shields.io/gem/v/enummer?color=green)](https://rubygems.org/gems/enummer)
[![Codecov](https://img.shields.io/codecov/c/github/shkm/enummer/main)](https://app.codecov.io/gh/shkm/enummer)
[![Licence](https://img.shields.io/github/license/shkm/enummer)](https://github.com/shkm/enummer/blob/main/MIT-LICENSE)
[![Documentation](https://img.shields.io/badge/yard-docs-informational)](https://www.rubydoc.info/github/shkm/enummer/main)

Enummer is a lightweight answer for adding enums with multiple values to Rails, with a similar syntax to Rails' built-in `enum`. It officially supports only PostgreSQL and recent Rails versions because I don't really care about anything else, but PRs are welcome.

## Installation
Add `gem "enummer"` to your Gemfile and `bundle`.

## Usage

### Setup
Create a migration for a bitstring that looks something like this:

```ruby
class CreateUsers < ActiveRecord::Migration[7.0]
  def change
    create_table :users do |t|
      t.column :permissions, "bit(8)"
    end
  end
end
```

Note that the limit is currently **required** — do not use variable bit fields. 1 bit gets you 1 option.

**TODO**: a generator. Because they generate things

Now set up enummer with the available values in your model:

```ruby
enummer permissions: %i[read write execute]
```

### Scopes

Scopes will now be provided for `<option>` and `not_<option>`.

```ruby
User.read
User.not_read
User.write
User.not_write
User.execute
User.not_execute
```

### Getter methods

Simply calling the instance method for the column will return an array of options. Question mark methods are also provided.

```ruby
user = User.last

user.permissions # => [:read, :write]

user.read? # => true
user.write? # => true
user.execute? # => false
```

### Setter methods

Options can be set with an array of symbols or via bang methods. Bang methods will additionally persist the changes.

```ruby
user.update(permissions: %i[read write])
user.write!
```

## Usage outside of Rails
lol stop

## Contributing
Make an issue / PR and we'll see.

## Alternatives
- [flag_shih_tzu](https://github.com/pboling/flag_shih_tzu)
- Lots of booleans
- DB Arrays

## License
The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
