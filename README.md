# Dependencies
* Ruby (2.1.0+)
* Bundle

# Setup
* Clone this repository with Git. Run 
  `git clone https://github.com/aileenzeng/aileenzeng.github.io.git`
* Run `bundle install` in the root directory to get dependencies.

# Local Testing
* Run `bundle exec jekyll serve`. This starts the local server and watches for changes in the 
  website.
* Run `jekyll build` if there are any changes to `_config.yml`. Changes to the config file do not 
  automatically show up on the local server and will need to be manually updated. You will need to 
  restart the local server after building.
* Open `http://0.0.0.0:4000/` in your web browser.

If all goes correctly, your terminal should look something like this:
```
aileen$ bundle exec jekyll serve

( some messages... )

Server address: http://127.0.0.1:4000/
Server running... press ctrl-c to stop.
```

# Misc
## Killing a process at a port
Sometimes you won't be able to start the website because port 4000 will already be in use. Here's
one way of fixing this:

```
lsof -i :4000
kill -9 <PID>
```

Example:
```
aileen$ lsof -i :4000

COMMAND   PID   USER   ...
ruby    12357 aileen   ...

aileen$ kill -9 12357
```