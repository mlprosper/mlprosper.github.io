---
layout: post
title: "Creating a Custom Cookbook for AWS OpsWorks"
---

AWS OpsWorks provides a number layers for common application environments like Rails, Haproxy, Node.js and others, but often these layers need additional configuration in order to work properly. In our case, we needed to add a second connection string to the database.yml of our Rails app.

The Chef recipe built-into OpsWorks doesn't support this by default. The recipe will build the database.yml, but only for a single database.  Here's the json to specify the connection details: 

    {
      "deploy": {
           "pals_api": {
             "database": {
                "adapter": "sqlserver",
                "host": "ec2-58-65-254.compute-1.amazonaws.com",
                "database": "my_db",
                "username": "my_username",
                "password": "my_password"
             }
          }
       }
    }

This JSON is used in a Chef ERB template to create the database.yml. The [AWS Docs](http://docs.aws.amazon.com/opsworks/latest/userguide/workingcookbook-template-override.html) cover how custom templates are to be used. This source code is [here](https://github.com/aws/opsworks-cookbooks/blob/master-chef-11.4/rails/templates/default/database.yml.erb):

    <% (['development', 'production'] + [@environment]).uniq.each do |env| -%>
    <%= env %>:
      adapter: <%= @database[:adapter].to_s.inspect %>
      database: <%= @database[:database].to_s.inspect %>
      encoding: <%= (@database[:encoding] || 'utf8').to_s.inspect %>
      host: <%= (@database[:host] || 'localhost').to_s.inspect %>
      username: <%= @database[:username].to_s.inspect %>
      password: <%= @database[:password].to_s.inspect %>
      reconnect: <%= @database[:reconnect] ? 'true' : 'false' %>
    <% if @database[:port] -%>
      port: <%= @database[:port].to_i.inspect %>
    <% end -%>

    <% end %>

Since we needed to specify additional databases, I thought I could just create a new "database" key in the JSON. Nope - the database key in the JSON is expected to be a single entity that is used to verify the type of database adapter during Rails setup. See [here](https://github.com/aws/opsworks-cookbooks/blob/master-chef-11.4/rails/libraries/rails_configuration.rb#L8) for more details.

To work around it, we just nested the credentials in the same JSON:

    {
      "deploy": {
           "pals_api": {
             "database": {
                "adapter": "sqlserver",
                "host": "ec2-58-65-254.compute-1.amazonaws.com",
                "database": "my_db",
                "username": "my_username",
                "password": "my_password"
                "additional_database": {
                  "name": "xml",
                  "host": "ec2-58-65-254.compute-1.amazonaws.com",
                  "database": "my_db",
                  "username": "my_username",
                  "password": "my_password"
                }
             }
          }
       }
    } 

We forked the opsworks-cookbooks repository and created a custom template. Be careful, follow the directions and only include the template file in your repository, as we did here:

[](https://github.com/CaseNEX/opsworks-cookbooks/tree/master-chef-11.4)

The directory structure must also match that in the OpsWorks cookbook.  Once you do that, point the layer to your custom-cookbook and AWS will run your template instead of the built-in template.

![s](https://dl.dropboxusercontent.com/u/11024433/Screenshots/2014-01-10_16-25-21.png)

Here's our database.yml.erb chef template for reference:

    <% (['development', 'production'] + [@environment]).uniq.each do |env| -%>

    <%= env %>:
      adapter: <%= @database[:adapter].to_s.inspect %>
      database: <%= @database[:database].to_s.inspect %>
      encoding: <%= (@database[:encoding] || 'utf8').to_s.inspect %>
      host: <%= (@database[:host] || 'localhost').to_s.inspect %>
      username: <%= @database[:username].to_s.inspect %>
      password: <%= @database[:password].to_s.inspect %>
      reconnect: <%= @database[:reconnect] ? 'true' : 'false' %>
    <% if @database[:port] -%>
      port: <%= @database[:port].to_i.inspect %>
    <% end -%>

    <% @db = @database[:additional_database] %>
    <% if @db -%>
    <%= "#{@db[:name]}_" if @db[:name] %><%= env %>:
      adapter: <%= @db[:adapter].to_s.inspect %>
      database: <%= @db[:database].to_s.inspect %>
      encoding: utf8
      host: <%= (@db[:host] || 'localhost').to_s.inspect %>
      username: <%= @db[:username].to_s.inspect %>
      password: <%= @db[:password].to_s.inspect %>
      reconnect: <%= @db[:reconnect] ? 'true' : 'false' %>
    <% if @db[:port] -%>
      port: <%= @db[:port].to_i.inspect %>
    <% end -%>
    <% end %>
    <% end %>

Not DRY, but it is clear that there are 2 separate entries in the database.yml.