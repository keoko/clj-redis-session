What is clj-redis-session
=========================

`clj-redis-session` uses redis as a Clojure/Ring's HTTP session
storage engine. What makes it different is its support for
hierarchical data, actually any *print-str*able clojure data.

Why clj-redis-session
=====================

The reason I wrote `clj-redis-session` is that the only redis-backed
sesssion store I could find, https://github.com/paraseba/rrss, doesn't
support hierarchical data structures, e.g. lists, maps.

Installation
============

Add

    [clj-redis-session "0.2.0"]

to `:dependencies` in your `project.clj`.

Usage
=====

`clj-redis-session` is a drop-in replacement for Ring native stores:

    (ns hello
      (:use [clj-redis-session.core :only [redis-store]]
            [clj-redis.client :only [redis])

    (def store (redis/init {:url "redis://127.0.0.1:6379"}))
    (def app
      (-> ....
          ... other middlewares ...
          (wrap-session {:store (redis-store store)})
          ....))

Want sessions to automatically expire?

    # expire after 12 hours
    (wrap-session your-app {:store (redis-store store {:expire-secs (* 3600 12)})})

You can also change the prefix (default to `session`) for the keys in
redis:

    (wrap-session your-app {:store (redis-store store {:prefix "i-am-prefix"})})

License
=======

Copyright (C) 2011 Wu Zhe <wu@madk.org>

Distributed under the Eclipse Public License, the same as Clojure.
