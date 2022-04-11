# ActiveAdmin::Globalize

Makes it easy to translate your resource fields.

[![Gem Version](https://badge.fury.io/rb/activeadmin-globalize.svg)](http://badge.fury.io/rb/activeadmin-globalize)
[![Build Status](https://travis-ci.org/fabn/activeadmin-globalize.svg?branch=develop)](https://travis-ci.org/fabn/activeadmin-globalize)

## Help Needed

Looking for maintainers. See https://github.com/fabn/activeadmin-globalize/issues/26

## Installation

Current released version on rubygems is `1.0.0.pre`, I won't call it 1.0.0 until some issues has been solved, 
but, as reported in [this PR](https://github.com/fabn/activeadmin-globalize/pull/25) it should work with both
AA 1.0.0 and AA 1.1.0. 

Current version targets Rails 4 and greater and ActiveAdmin >= 1.0.0.

```ruby
gem 'activeadmin-globalize', '~> 1.0.0.pre'
```

Previous version with support for Rails 3 is maintained in branch [support/0.6.x](https://github.com/fabn/activeadmin-globalize/tree/support/0.6.x)

## Require Assets

- active_admin.js: `//= require active_admin/active_admin_globalize.js`
- active_admin.css: `*= require active_admin/active_admin_globalize`

## Your model

```ruby
active_admin_translates :title, :description do
  validates_presence_of :title
end
```
## In your Active Admin resource definition

```ruby

# For usage with strong parameters you'll need to permit them
permit_params translations_attributes: [:id, :locale, :title, :description, :_destroy]

index do
  # textual translation status
  translation_status
  # or with flag icons
  translation_status_flags
  # ...
  actions
end

form do |f|
  # ...
  f.translated_inputs "Translated fields", switch_locale: false do |t|
    t.input :title
    t.input :description
  end
  # ...
end

# You can also set locales to show in tabs
# For example we want to show English translation fields without tab, and want to show other languages within tabs
form do |f|
  # ...
  f.inputs do
    Globalize.with_locale(:en) do
      f.input :title
    end
  end
  f.inputs "Translated fields" do
    f.translated_inputs 'ignored title', switch_locale: false, available_locales: (I18n.available_locales - [:en]) do |t|
      t.input :title
      t.input :description
    end
  end
  # ...
end

# You can also set default language tab
# For example we want to make Bengali translation tab as default
form do |f|
  # ...
  f.inputs "Translated fields" do
    f.translated_inputs 'ignored title', switch_locale: false, default_locale: :bn do |t|
      t.input :title
      t.input :description
    end
  end
  # ...
end

```
If `switch_locale` is set, each tab will be rendered switching locale.


## Hints

To use the dashed locale keys as 'pt-BR' or 'pt-PT' you need to convert a string
to symbol (in application.rb)

```ruby
  config.i18n.available_locales = [:en, :it, :de, :es, :"pt-BR"]
```

## Credits

This work is based on original idea by [@stefanoverna](https://github.com/stefanoverna/activeadmin-globalize),
 I needed it for AA 0.6.x so I forked the original project and expanded it with more features.
