---
layout: post
title: "Progressbar Upload with Refile Gem"
date: 2015-03-03 23:30:07 +0700
comments: true
categories: [ruby, javascript]
---
There is a ruby gem library for file upload called [refile](https://github.com/refile/refile).
One of features that i like is built in javascript library, which is contains several custom DOM events.
These events also can be listen, so you can provide logic easily around file upload lifecycle process.

By default refile provide helpers to built UI element of file input field:

```ruby
= f.attachment_field :file_field
```

From that snippet above refile won't use built in javascript, to enable it just add an option called **direct** with true.
<!-- MORE -->
```ruby
= f.attachment_field :file_field, direct: true
```
Refile provide several events which can attached into form upload element, for attaching the events you can use **addEventListener** :
```javascript
form.addEventListener('upload:start', function(e){});
form.addEventListener('upload:progress', function(e){});
form.addEventListener('upload:success', function(e){});
form.addEventListener('upload:complete', function(e){});
form.addEventListener('upload:failure', function(e){});
```
Because we focused only progressbar upload, lets construct the form to using twitter bootstrap's progressbar, currently i'm using slim-template for view.
```ruby
= form_for @object, html: {class: 'form-horizontal'} do |f|
  ...
  .form-group
    = f.label :file_field, class: 'col-sm-3 control-label'
    .col-sm-7
      = f.attachment_field :file_field, class: 'form-control', direct: true
      #progress-bar-container.hidden
        .progress.progress-striped
          .progress-bar.progress-bar-info style="width: 0%"
  ...
```
Next lets write the javascript for calculate the progress and manipulate the UI progressbar.
We can utilize **upload:progress** event for this purpose.
```javascript
FileUploader = {
  init: function(){
    var form = document.querySelector('form');
    form.addEventListener('upload:progress', function(event){
      this.doProgress(event);
    }.bind(this));
  },

  doProgress: function(event){
    var detail = event.detail;
    var percentage = Math.round( (detail.loaded / detail.total) * 100);
    $('.progress-bar').css('width', percentage+'%');
  }
}

...

$(document).ready(function(){
  Object.create(FileUploader).init();
});
```
Finally this is our progressbar using refile gem, for more information you can read on [refile](https://github.com/refile/refile).
