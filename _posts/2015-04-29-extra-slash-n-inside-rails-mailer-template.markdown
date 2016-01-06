---
layout: post
title: "Extra '\\n' inside rails mailer template"
date: 2015-04-29 00:21:02 +0700
comments: true
categories: 
---
When i work with the email stuff, i accidentally found problem when email template was already sent by Mandrill SMTP.
For example in my apps there is a feature to reset password, which is of course inside the email contains **reset password** link.

For securing the reset password link it must supply a params called **reset_password_token**. The problem found when the reset password link doesnt
work, because its always wrong redirect. Can you spot the problem ?.

```html
<a href="http://example.com/users/password/edit?reset_password_to%20ken=asdf1234">change password</a>
```

<!-- MORE -->
Accidentally the links params contains extra **\\n**. This kind of issue with html frameworks that generate HTML with no true line breaks.
Based on [rfc5321](https://www.ietf.org/rfc/rfc5321.txt) **4.5.3.1.6.  Text Line** its state :

> The maximum total length of a text line including the <CRLF> is 1000
> octets (not counting the leading dot duplicated for transparency).

Hell yeah because i'm using SLIM for mailer template, in production SLIM compiles as a single-line by default ``pretty: false`` for optimization. 
We can solve it by toggle the **pretty** option to true, see [slim config](http://www.rubydoc.info/gems/slim/frames#Configuring_Slim). 


But this kind of solution will be sacrifice to other page except the mailer template not being optimized. 
Instead using slim for mailer template, just use **ERB** so it will break every new line.
