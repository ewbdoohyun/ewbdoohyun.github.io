---
layout: post
comments: true
title: 테스트 자동화 - guard
category: [dev, rails]
---

### Guard

`Gemfile`

      group :test do
        gem 'guard-rspec'
      end

`bundle exec guard init rspec`


`Guardfile`

    # Note: The cmd option is now required due to the increasing number of ways
    #       rspec may be run, below are examples of the most common uses.
    #  * bundler: 'bundle exec rspec'
    #  * bundler binstubs: 'bin/rspec'
    #  * spring: 'bin/rspec' (This will use spring if running and you have
    #                          installed the spring binstubs per the docs)
    #  * zeus: 'zeus rspec' (requires the server to be started separately)
    #  * 'just' rspec: 'rspec'    

    guard :rspec, cmd: "zeus rspec" do
      require "guard/rspec/dsl"
      dsl = Guard::RSpec::Dsl.new(self)
    
      # RSpec files
      rspec = dsl.rspec
      watch(rspec.spec_helper) { rspec.spec_dir }
      watch(rspec.spec_support) { rspec.spec_dir }
      watch(rspec.spec_files)
    
      # Ruby files
      ruby = dsl.ruby
      dsl.watch_spec_files_for(ruby.lib_files)
    
      # Rails files
      rails = dsl.rails(view_extensions: %w(erb haml slim))
      dsl.watch_spec_files_for(rails.app_files)
      dsl.watch_spec_files_for(rails.views)
    
      watch(rails.controllers) do |m|
        [
          rspec.spec.call("routing/#{m[1]}_routing"),
          rspec.spec.call("controllers/#{m[1]}_controller"),
          rspec.spec.call("acceptance/#{m[1]}")
        ]
      end
    
      # Rails config changes
      watch(rails.spec_helper)     { rspec.spec_dir }
      watch(rails.routes)          { "#{rspec.spec_dir}/routing" }
      watch(rails.app_controller)  { "#{rspec.spec_dir}/controllers" }
    
      # Capybara features specs
      watch(rails.view_dirs)     { |m| rspec.spec.call("features/#{m[1]}") }
      watch(rails.layouts)       { |m| rspec.spec.call("features/#{m[1]}") }
    
      # Turnip features and steps
      watch(%r{^spec/acceptance/(.+)\.feature$})
      watch(%r{^spec/acceptance/steps/(.+)_steps\.rb$}) do |m|
        Dir[File.join("**/#{m[1]}.feature")][0] || "spec/acceptance"
      end
    end
        


`.gitignore`

    /Spring


---


[1]: https://www.railstutorial.org/book/_single-page#sec-guard
