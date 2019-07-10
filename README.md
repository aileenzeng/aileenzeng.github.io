# Dependencies
* Ruby (2.1.0+)
* Bundle

# Setup
* Run `bundle install` in the root directory to get dependencies.

# Local Testing
* Run `bundle exec jekyll serve`. This starts the local server and watches for changes in the 
  website.
* Run `jekyll build` if there are any changes to `_config.yml`. Changes to the config file do not 
  automatically show up on the local server and will need to be manually updated. You will need to 
  restart the local server after building.
* Open `http://0.0.0.0:4000/` in your web browser.