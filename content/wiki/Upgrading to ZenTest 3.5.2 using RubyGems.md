---
tags: zentest updates
cache_breaker: 1
---

# Upgrade

    sudo gem install ZenTest

## Output

    Successfully installed ZenTest-3.5.2
    Installing ri documentation for ZenTest-3.5.2...
    Installing RDoc documentation for ZenTest-3.5.2...

# Update [FastRI](/wiki/FastRI) index

    fastri-server -b

## Output

    Indexing RI docs for ZenTest version 3.5.2.
    Indexing RI docs for actionmailer version 1.3.3.
    Indexing RI docs for actionpack version 1.13.3.
    Indexing RI docs for actionwebservice version 1.2.3.
    Indexing RI docs for activerecord version 1.15.3.
    Indexing RI docs for activesupport version 1.4.2.
    Indexing RI docs for dhaka version 2.1.0.
    Indexing RI docs for diff-lcs version 1.1.2.
    Indexing RI docs for fastri version 0.3.0.1.
    Indexing RI docs for hoe version 1.2.0.
    Indexing RI docs for model_security_generator version 0.0.9.
    Indexing RI docs for rake version 0.7.3.
    Indexing RI docs for rcodetools version 0.5.0.0.
    Indexing RI docs for rcov version 0.8.0.2.
    Indexing RI docs for rspec version 0.9.2.
    Indexing RI docs for ruby-debug version 0.9.3.
    Indexing RI docs for ruby-debug-base version 0.9.3.
    Indexing RI docs for ruby-prof version 0.4.1.
    Indexing RI docs for rubyforge version 0.4.1.
    Indexing RI docs for rubygems version 0.9.2.
    Indexing RI docs for sqlite3-ruby version 1.2.1.
    Indexing RI docs for walrus version 0.1.
    Indexing RI docs for zentest version 3.5.0.
    Building index.
    Indexed:
    * 12060 methods
    * 2404 classes/modules
    Needed 3.837212 seconds

# Changes

-   Patch up Rails fixture defaults for Test::Rails::TestCase.
-   Session now properly hooked up to controllers.
-   ruby 1.8.6 has a bug on 'raise Interrupt' with no args. Fixed on both sides.
-   Fixed redgreen to work with new getc/putc-based output. (from Finn Smith)

# See also

-   Release notes: <http://rubyforge.org/frs/shownotes.php?release_id=11408>
