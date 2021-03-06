## Goalie

Goalie is a flexible dynamic error response renderer for Rails built
on Rack and Rails Engines. It provides the same default error pages as
Rails, but allows you to easily customize them with *dynamic*
content. This means you can use your application layout, have
different error pages for different subdomains, and do all sorts nice
things.

## Installation

    gem install goalie

After you install it and add it to your `Gemfile`, you have to require
it together with Rails' frameworks at the top of your
`config/application.rb` file:

    require 'goalie/rails'

This will remove Rails' default exception renderer middleware
(`ShowExceptions`) and use Goalie's instead. Unless you have custom
static pages in your `public` directory (which we plan to support
later), this will be a drop-in replacement.

## Customization

### Controllers

The public (production) rescuing of errors is done by the
`PublicErrorsController` found in Goalie's `app/controllers`
directory. If you create a controller with the same name, it will
automatically be used instead of Goalie's. All it needs to do is
support the following actions:

 * `internal_server_error`
 * `not_found`
 * `unprocessable_entity`
 * `conflict`
 * `method_not_allowed`
 * `not_implemented`

If you don't actually need a separate action for each of these errors,
you can redirect them to others, for example, with:

    def unprocessable_entity
      render :action => 'internal_server_error'
    end

### Views

You can also customize only the views and use Goalie's default
controller. All you need is to have inside `app/views/public_errors`
views with the same names as the actions listed above. 

Besides the standard stuff that Rails makes available to views, you will also have
access to the following instance variables if you include the Goalie::ErrorDetails module:

 * `@request`
 * `@exception`
 * `@application_trace`
 * `@framework_trace`
 * `@full_trace`

Be VERY careful when using this in production, as you could expose
sensitive information inside the request and exception. Generally, you
probably shouldn't use these variables at all. The only place it makes
sense is to have a more detailed error screen for admins or other
high-level users.  By default these details are not available to the public errors views
but can be made available if you override the `PublicErrorsController` and 
include the Goalie::ErrorDetails module.



## Credit

Goalie copies a lot of code and ideas from:
 * Rails' `ShowExceptions` middleware
 * Rails' default error views
 * Rails' exception_notification plugin

Which are mostly the work of [Joshua Peek](http://joshpeek.com), with
help from various contributors. We're highly indebted to them and
thank them a lot for their work.

## Contributions

Any form of feedback, patches, issues, and documentation are highly
appreciated.

## License

MIT license. Copyright 2010 [Helder Ribeiro](http://helderribeiro.net).

