# Snipp
[![Gem Version](https://badge.fury.io/rb/snipp.png)](http://badge.fury.io/rb/snipp)
[![Coverage Status](https://coveralls.io/repos/yulii/snipp/badge.png?branch=master)](https://coveralls.io/r/yulii/snipp)
[![Build Status](https://travis-ci.org/yulii/snipp.png)](https://travis-ci.org/yulii/snipp)

Search Engine Optimization (SEO) helpers for Ruby on Rails

## Supported
- HTML meta tags
- Rich snippets and structured data

## Installation
Add the "snipp" to your application's Gemfile:

```ruby
gem 'snipp'
```
And run `bundle install` command.

## HTML Meta Tags
Call `html_meta_tags` in a view template.
```html+ruby
<!DOCTYPE html>
<html>
<head>
  <meta charset='utf-8'>
  <%= html_meta_tags %>
  <%= stylesheet_link_tag :all %>
  <%= javascript_include_tag :defaults %>
  <%= csrf_meta_tag %>
  ...
```

### Set content attribute values
The priority of setting values

1. Call `set_html_meta` 
2. Use I18n dictionary (controller action scope)
3. Use I18n dictionary (default scope)
4. Set Snipp.config.html_meta_tags

#### Use helper
Call `set_html_meta` in a view.
```ruby
set_html_meta({
  title: "Page Title",
  description: "Page Description",
  og: {
    title: "Title for Social Media"
  },
  link: {
    canonical: "https://github.com/yulii/snipp"
  }
}, {
  arg: "Option Values for I18n",
  page: params[:page]
})
```

#### User I18n dictionary files
for each controller action
```ruby
I18n.t(:title, scope: [:views, params[:controller], params[:action], :meta])
```

default values
```ruby
I18n.t(:title, scope: [:default, :meta])
```

For more information
- read erb files in [`app/view/snipp/html.html.erb`](https://github.com/yulii/snipp/tree/master/app/views/snipp)
- read locale files in [`config/locales`](https://github.com/yulii/snipp/tree/master/app/views/snipp)


## Rich Snippets
Call `item_tag` like `content_tag`

```html+ruby
<%= item_tag :div, scope: true, type: :review do %>
  <%= item_tag :span, 'Snipp', prop: :itemreviewd %>
  Reviewed by <%= item_tag :span, '@yulii', prop: :reviewer %> on
  <%= item_tag :time, 'April 1', prop: :dtreviewd, datetime: "2013-04-01" %>.
  <%= item_tag :span, 'Review summary...', prop: :summary %>
  <%= item_tag :span, 'Review description...', prop: :description %>
  Rating: <%= item_tag :span, '4.5', prop: :rating %>
<% end %>
```

This would output

```html
<div itemscope itemtype="http://data-vocabulary.org/Review">
  <span itemprop="itemreviewed">Snipp</span>
  Reviewed by <span itemprop="reviewer">@yulii</span> on
  <time itemprop="dtreviewed" datetime="2013-04-01">April 1</time>.
  <span itemprop="summary">Review summary...</span>
  <span itemprop="description">Review description...</span>
  Rating: <span itemprop="rating">4.5</span>
</div>
```

### Breadcrumbs
1. Define naming routes. You can specify a name for any route using the :as option in Rails Application.
2. Setup the Rails Application for Internationalization (i18n).
3. Call `breadcrumb` method.

```html+ruby
<%= breadcrumb [:index, :foods, :fruits] %>
```
This would output something like `Top > Foods > Fruits`, if you will set labels in your `Rails.root/config/locales`
```yaml
en:
  views:
    breadcrumb:
      # Sample
      index: "Top"
      food: "Foods"
      food_fruit: "Fruits"
```
Link to `root_path`, `food_path`, `food_fruit_path`.


#### Change the separator
Use the :s or :separator option.

```html+ruby
<%= breadcrumb [:index, :foods, :fruits], s: "/" %>
```

#### Use variables and Customize labels

```html+ruby
<%= breadcrumb [:index, :foods, :fruits, { path: fruits_color_path('Red'), label: 'Red' }, { path: food_path(color: 'Red', name: 'Apple'), label: 'Apple' }], s: "/" %>
```

For more information, read erb files in [`app/view/snipp/`](https://github.com/yulii/snipp/tree/master/app/views/snipp)

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## LICENSE
(The MIT License)

Copyright © 2013 yulii. See LICENSE.txt for further details.
