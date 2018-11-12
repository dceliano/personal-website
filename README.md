# Introduction
This repo contains all the files for my personal website. The actual files which can be accessed on my website are in the indigo directory.

To Deploy to S3 (to be able to access it using domcc3.com):

	./deploy.sh

To Deploy Locally (to be able to access it using localhost):

	./test_deploy.sh

# Building Jekyll (15 Oct 2018)
This website uses Jekyll, with the [Indigo](https://github.com/sergiokopplin/indigo) template. Here were the commands I used to copy the template and build it myself:

	git clone https://github.com/sergiokopplin/indigo.git
	cd indigo
	bundle install		#installs any required gems (dependencies)
	bundle exec /Library/Ruby/Gems/2.3.0/gems/jekyll-3.8.4/exe/jekyll build		#jekyll will not work if you don't have the required gems in the Gemfile.
	cd _site
	open index.html

"bundle install" was breaking when it tried to install nokogiri, which was required for github-pages. The solution per [here](https://stackoverflow.com/questions/40038953/how-to-install-nokogiri-on-mac-os-sierra-10-12) was to type:

	sudo gem install nokogiri -v 1.8.5 -- --use-system-libraries=true --with-xml2-include="$(xcrun --show-sdk-path)"/usr/include/libxml2

## Final command, as of 12 Nov 2018:
The final command (after reinstalling nokogiri and other bundles. Ruby gives me tons of problems -- especially when I update my version of MacOS.):

	bundle exec jekyll build

# A custom favorite icon
The favicon can be generated from any image. This website https://realfavicongenerator.net took care of all the magic for me, and I copied the format from the indigo template.