---
layout: post
title: "CarrierWave uploader for tableless model in Ruby on Rails"
date: 2013-11-18 23:57:05 +0000
comments: true
categories: [ruby, rails, carrierwave]
---
In this post i want to share my experience in implementing Carrierwave for tableless model, for example i've model User :
```ruby
class User
  attr_accessor :id, :name, :avatar
  ...
end
```
<!-- MORE -->
I need to store user's avatar using carrierwave, in this case i create **AvatarUploader**, the problem is how i store avatar from instantiating User model ? If your class is subclass of ActiveRecord::Base, you can use **mount_uploader**
method, but if you have model like User which is not subclass of ActiveRecord::Base you will get exception :

  undefined method `mount_uploader' for User:Class

Based on [carrierwave documentation](http://carrierwave.rubyforge.org/rdoc/) you can use [mount_uploader](http://carrierwave.rubyforge.org/rdoc/classes/CarrierWave/Mount.html#M000027) if your class extend **CarrierWave::Mount** module and your class will have capability to store a file using instance method `store_(mounted_field)!` or `cache_(mounted_field)!`. So lets refactor our User model :
```ruby
class User
  extend CarrierWave::Mount
  attr_accessor :id, :name, :avatar
  mount_uploader :avatar, AvatarUploader
  ...
  def save
    self.store_avatar!
  end
  ...
end
```

And you can test from rails console :

  u = User.new(id:1, name:"swagger")
  u.avatar = File.open(some_avatar_image_file_path)
  u.save

from rails console, a some version of avatar will created in **public/tmp/timestamp/** when  you assign image file to avatar field, when method **save** executed it will be move to `public/uploads/#{model.class.to_s.underscore}/#{mounted_as}/#{model.id}`
