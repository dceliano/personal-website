# Introduction
This repo contains all the files for my personal website. The actual files which are pushed to S3 and which can be accessed on my website are in the `indigo/_site` directory.

To deploy the website to S3 (to be able to access it using domcc3.com), `apt-get install awscli` then run `aws s3 sync _site s3://domcc3.com` (after setting up the keys using `aws configure`).

To host the website locally, host the contents of `indigo/_site` using any local web server, such as `python3 -m http.server`.

# Building the website (as of 18 Jan 2021)

The following instructions will make the Ruby-based build process work properly on a freshly installed Debian 10 system - steps should be similar on any Linux system:

```
apt-get install ruby gcc make g++ zlib1g-dev
(I have no clue why you need so many dependencies, but you do.)
gem install bundler
export LC_ALL="C.UTF-8"
export LANG="en_US.UTF-8"
export LANGUAGE="en_US.UTF-8"
(I was getting a UTF-8 error, and setting those environment variables fixed it.)
And finally:
cd personal-website/indigo
bundle exec jekyll build
```

# The Favorite Icon
Favicons can be generated from any image, using websites such as [this one](https://realfavicongenerator.net), which I used.
