---
layout: post
title: "Rails Caching"
date: 2014-03-19 09:27:04 -0400
comments: true
categories: caching rails
published: true
---

#Caching in Rails

##What is Caching?
It is a tool that allows you to avoid extensive database querying on a page by storing elements of a page in memory and retrieving that memory store each time that page is visited. It enables faster page loading on refresh, and saves resources.

##Cache Types in Rails
###Page Caching
This is a Rails mechanism that allows a request for a generated page to be fulfilled by the webserver. This has limited uses, and can't be used with pages that have `before` filters (such as pages that require authentication). It also requires that cache expiration be set explicitly. Since Rails 4, the page caching feature has been removed into a separate gem called `actionpack-page`.

###Action Caching
Action caching is used where page caching can't be utilized -- such as with pages that require authentication. It is very similar to page caching, except that the incoming request hits the Rails stack so that `before` filters can be executed before the cache is served. Since Rails 4, the action caching feature has been removed into a separate gem called `actionpack-action`.

###Fragment Caching
Fragment caching allows a fragment of view logic to be wrapped inside of a cache block and served out of the cache store when the next page request comes calling. Basically, the cache block is wrapped around logic inside of your view, and that cached view logic will be served to the page view until it expires. Then the cache process will start over again.

##Cache Setup
###Configuration Settings
You can set up your app's default cache store by calling `config.cache_store=` inside `config/application.rb` or inside of your environment files in `config/environments/*.rb`.

###Cache::Store
This is the foundation for interacting with the Rails cache. The class in Rails is provided via `ActiveSupport::Cache::Store`. There are four primary methods: `read`, `write`, `delete`, `exist?`, and `fetch`. The `fetch` method takes a block and will return an existing cache, or it will evaluate the block and write the result to the cache if a cache doesn't exist previously.

There are four options that can be passed in to the `config.cache_store=` configuration. They are:
- `:namespace` - Option is used to create a namespace within the cache store (useful when cache is shared with other applications). Default is the application name and Rails environment.
- `:compress` - Used to indicate that compression should be used in the cache (useful for transferring large caches)
- `:compress_threshold` - Used int conjunction with `:compress` to indicate a threshold under which caches should not be compressed (default is 16 kilobytes)
- `:expires_in` - Sets an expiration time in seconds
- `:race_condition_ttl` - Used in conjunction with `:expires_in` option to prevent race conditions when a cache expires (basically prevents multiple processes from regenerating cache entries simultaneously)

###Cache::MemoryStore
This stores cache entries in memory. This has a size limit specified by the `:size` option (default size is 32 megabytes). When the cache exceeds the size limit, a cleanup will occur. This is not ideal for large app deployments, and typically works best for small, low traffic sites.

This has to be specified in configurations via:
- `config.cache_store = :memory_store, { size: 128.megabytes }`

##Heroku Cache Configuration Setup
In order to enable caching with Heroku, it works best with `Memcachier`, which is a Heroku add-on. `Memcachier` essentially manages and scales clusters of memcache servers for Heroku apps. See the link below for setup instructions.

- [Link to Heroku Memcachier Setup]("https://devcenter.heroku.com/articles/rack-cache-memcached-rails31#configure-rails-cache-store")
