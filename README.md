Feedback Plugin for Rails _(for Rails 3.0.x)_
=============================================

Feedback is a Ruby on Rails plugin that adds an ajax-based 
overlay feedback form triggered by a side-screen tab or a link. 
The result of the feedback form is sent through a ActionMailer.

The plugin was inspired from the "feedback" widget of 
GetSatisfaction.com and UserVoice.com. 

The plugin uses a generator to generate boiler-plate Javascript,
CSS, HTML, and ruby code into your application.
It sets up a ready to use feedback form (with minimal styling)
that you can further customize.

Features
--------

* Helper to display a sticky "feedback" tab.
* Display feedback form as an overlay with Ajax.
* Sends feedback result by email through an ActionMailer.
* Uses a generator such that you can customize the code and style to fit your app.
* Generates unit and functional tests.
* Works with Prototype and JQuery, choose through a generators flag.
* Currently supports IE6, IE7, IE8, FF2, FF3, Chrome2 and SafariX


Motivation
----------

Collecting feedback from users is really important. Third-party services
such as GetSatisfaction.com and UserVoice.com are awesome tools to manage your feedback. 
However, we found out that in many instances such as for small sites or back-ends, we wanted 
a similar feedback form integration/experience but simpler, local, and customized.


Install and Getting Started
---------------------------

This version offers support for Rails 3.0.x.  
Support for Rails 2.3.x can be found by installing Feedback 1.0.x from the v1.0 branch.  
Support for Rails 3.1.x and newer can be found in the master branch.

Install the plugin:

    ruby script/plugin install git://github.com/dmfrancisco/feedback.git -r v2.0

Run the generator:

    ruby script/generate feedback_form

By default it will generate the Prototype code, you can use jQuery if you prefer:

    ruby script/generate feedback_form --jquery


Make sure Prototype is included in your header:

    <%= javascript_include_tag :defaults %>


Include the feedback.css and the prototype.feedback.js files into your header.
There is a helper to do that:

    <%= feedback_includes %>


To add a sticky 'feedback' tab to your site, in the header place:

    <%= feedback_tab(:position => 'top') %>


Configure the action mailer in app/models/feedback_mailer.rb

    def feedback(feedback)
      @recipients = 'you@yoursite.com'
      @from = 'system@yoursite.com'
      @subject = "[Feedback for YourSite.com] #{feedback.subject}"
      ...


How-to's
--------

### How to customize the position of the feedback tab?

The feedback_tab helper takes the option "position" which has four different values (top|bottom|left|right)
that corresponds to class in the feedback.css file. You can fine tune the position by changing the left/right, top/bottom
attributes of those css classes.

### How to trigger the feedback form from a custom link?

The feedback_link(text, options={}) helper allows you the create a link that will trigger the feedback form.

    <%= feedback_link "Leave us feedback!" %>

Your layout header must also have the includes:

    <%= feedback_includes %>

### How to customize the email message?

Edit the /app/views/feedback_mailer/feedback.html.erb generated file in your app

### How to store the feedback results in the database?

Feedback generates a model that is not an active record model. In order to store the feedbacks in your database, make it
an active record model:

    class Feedback < ActiveRecord::Base
      validates_presence_of :comment
    end

You must also write a migration to create your table:

    class CreateFeedbacks < ActiveRecord::Migration
      def self.up
        create_table :feedbacks do |t|
          t.string :subject
          t.string :email
          t.text   :comment
          t.timestamps
        end
      end
    
      def self.down
        drop_table :feedbacks
      end
    end


Limitations (TODOs)
-------------------

* No unit tests for Javascript
* Feedback PNG with transparency supported by IE6
* Graceful fallback if Javascript is not enabled/supported
* More customization options through helpers: (overlay style, tab color, etc...)


Acknowledgements
----------------

Thanks to GetSatisfaction.com for the
idea of the "feedback" tab.

Part of this plugin was inspired from other software.
Thanks to their respective authors.
* Facebox: http://famspam.com/facebox
* restful_authentication: http://github.com/technoweenie/restful-authentication/tree/master


Author and main contributors
----------------------------

This project was created by [Jean-Sebastien Boulanger](https://github.com/jsboulanger/feedback).  
Changes for the Rails 3.0 compatible version were made by [Alkesh Vaghmaria](https://github.com/alkesh/feedback).  
Final adjustments for the Rails 3.1 compatible version and the demo were made by [David Francisco](https://github.com/dmfrancisco/feedback).

---

Copyright (c) 2009 Jean-Sebastien Boulanger <jsboulanger@gmail.com>, released under the MIT license

[http://jsboulanger.com](http://jsboulanger.com)
