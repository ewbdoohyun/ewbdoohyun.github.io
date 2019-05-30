---
title: Rails(레일즈) test with rspec
layout: post
---

- 테스트 하지 말아야 할 것.
    + ruby/rails에서 제공하는 method
    + 남이 만든
- 테스트 꼭 해야 할 것.
    + 니가 짠 method
    + business logic
    + 걍 ㅋ 니가 만든 것 다

**Setting**
- `Gemfile`
    {% highlight ruby %}
        group :test do
            gem 'guard-rspec'
            gem "faker", "~> 1.4.3"
            gem 'rspec-rails', '~> 3.5' 
            gem "factory_girl_rails", "~> 4.0"
    {% endhighlight %}
- `rspec-rails`
    + `rails generate rspec:install`
- `factory_girl_rails`
    + `spec_helper.rb`
        {% highlight ruby %}
            require 'factory_girl_rails'
            RSpec.configure do |config|
                ...
                config.include FactoryGirl::Syntax::Methods
                ...
        {% endhighlight %}
    + `spec/factories.rb`
        {% highlight ruby %}
            FactoryGirl.define do
              factory :item do
              end
            end
        {% endhighlight %}
- `guard-rspec`
    + `bundle exec guard init rspec`
- `faker`
    + 


**Controller**
- 원하는 Data를 생성하는지 테스트
    + `expect(assigns(:contacts)).to match_array([smith])``
- 원하는 View를 그리는지 테스트




