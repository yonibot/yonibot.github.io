---
layout: post
title: "Adding File Upload Functionality with the Carrierwave Gem"
date: 2013-08-11 17:12
comments: true
categories: 
---
Adding a file upload feature to an app is fairly simple with the popular
**Carrierwave** gem. And commercial file hosting is seamless as well
with **Amazon's Simple Storage Service (S3)**, which plays very nicely
with Carrierwave. I'll cover S3 integration in a later post. I've broken
down the process into 3 easy steps:

1. Add a file upload field in your view and a column to your model
------------------------------------------------------------------

The first step is to implement a file upload field in a form on your
app:
```ruby
form_for .... do   
  f.file_field :passport_picture 
  f.submit "Submit"`
```
You need a column called "passport\_picture" in your model, so generate
a migration and add in that column. We used a popular gem called
[Carrierwave][] to handle the file uploading, so let's add it and its
associated gems to the Gemfile:
` gem 'carrierwave' gem "fog", "~> 1.3.1" gem 'mini_magick'`

2. Make uploader classes for each file
--------------------------------------

Now we need to make an uploader class that contains uploading
instructions. Our uploader will instruct Carrierwave to use the
**MiniMagick** gem for image processing (optional):
<span style="text-decoration:underline;">app/uploaders/passport\_picture\_uploader.rb</span>
```ruby
class PassportPictureUploader < CarrierWave::Uploader::Base   
include CarrierWave::MiniMagick    
store: file   
process :resize_to_fill => [200, 200] end
```
One caveat here: you need to make sure ImageMagick is installed on your
computer, and this was not easy for me. You can install it using
Homebrew or Rvm if you don't have it, but google around for more
specific instructions.

3. Mount the uploaders in your model
------------------------------------

Now you need to tell your model to mount the uploader(s):
```ruby
class User < ActiveRecord::Base   
  mount_uploader :passport_picture, PassportPictureUploader 
end
```
And that's it! As an aside, I think this would be a good candidate to
shuffle off to a background process so that the user doesn't have to
wait until the file uploads for the form to submit. I'll talk about what
I've learned about background processes in a later post.

  [Carrierwave]: https://github.com/carrierwaveuploader/carrierwave