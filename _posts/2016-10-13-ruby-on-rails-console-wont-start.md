---
layout: post
title: "Ruby on Rails console won't start"
date: 2016-10-13 12:35:19 -0600
comments: true
published: true
tags: []
categories: Development
---

If you receive a similar message when you try to start your `rails console`:

```bash
❯ bundle exec rails console
/Users/mike/.rbenv/versions/2.2.2/lib/ruby/2.2.0/irb/completion.rb:9:in `require': dlopen(/Users/mike/.rbenv/versions/2.2.2/lib/ruby/2.2.0/x86_64-darwin15/readline.bundle, 9): Library not loaded: /usr/local/opt/readline/lib/libreadline.6.dylib (LoadError)
  Referenced from: /Users/mike/.rbenv/versions/2.2.2/lib/ruby/2.2.0/x86_64-darwin15/readline.bundle
  Reason: image not found - /Users/mike/.rbenv/versions/2.2.2/lib/ruby/2.2.0/x86_64-darwin15/readline.bundle
        from /Users/mike/.rbenv/versions/2.2.2/lib/ruby/2.2.0/irb/completion.rb:9:in `<top (required)>'
        from /Users/mike/.rbenv/versions/2.2.2/lib/ruby/gems/2.2.0/gems/railties-4.2.1/lib/rails/commands/console.rb:3:in `require'
        from /Users/mike/.rbenv/versions/2.2.2/lib/ruby/gems/2.2.0/gems/railties-4.2.1/lib/rails/commands/console.rb:3:in `<top (required)>'
        from /Users/mike/.rbenv/versions/2.2.2/lib/ruby/gems/2.2.0/gems/railties-4.2.1/lib/rails/commands/commands_tasks.rb:123:in `require'
        from /Users/mike/.rbenv/versions/2.2.2/lib/ruby/gems/2.2.0/gems/railties-4.2.1/lib/rails/commands/commands_tasks.rb:123:in `require_command!'
        from /Users/mike/.rbenv/versions/2.2.2/lib/ruby/gems/2.2.0/gems/railties-4.2.1/lib/rails/commands/commands_tasks.rb:58:in `console'
        from /Users/mike/.rbenv/versions/2.2.2/lib/ruby/gems/2.2.0/gems/railties-4.2.1/lib/rails/commands/commands_tasks.rb:39:in `run_command!'
        from /Users/mike/.rbenv/versions/2.2.2/lib/ruby/gems/2.2.0/gems/railties-4.2.1/lib/rails/commands.rb:17:in `<top (required)>'
        from bin/rails:4:in `require'
        from bin/rails:4:in `<main>'
```

Then you most likely have an issue with `readline`.

Install `readline` and `ruby-build`:

```bash
❯ brew install readline ruby-build
```

Then reinstall ruby (assuming you are using rbenv):

```bash
❯ env CONFIGURE_OPTS=--with-readline-dir=`brew --prefix readline` rbenv install 2.2.2
```

Then reset your gems:

```bash
❯ gem pristine --all
```

Enjoy your working `rails console`!

```bash
❯ bundle exec rails console
Loading development environment (Rails 4.2.1)
irb(main):001:0> puts "Hello World"
Hello World
=> nil
```
