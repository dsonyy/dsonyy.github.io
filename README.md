
# things i build

> Yo, this is my personal website where I post on things I build.


[**💡 Check out on szymon.blog**](https://szymon.blog/)


## Prerequisites

- Ruby `ruby -v`
- RubyGems `gem -v`
- GCC and Make `gcc -v && make -v`
- Jekyll bundler `gem install jekyll bundler`

```bash
sudo apt update
sudo apt install ruby ruby-dev
sudo apt install build-essential
gem install jekyll bundler
```

## Building

To build [Jekyll](https://jekyllrb.com/docs/installation/) site, run the following command:

```bash
bundle exec jekyll serve
```

To build the site and watch for changes, run the following command:

```bash
bundle exec jekyll serve --watch
```