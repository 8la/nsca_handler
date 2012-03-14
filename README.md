DESCRIPTION
============
Chef handler to use with the [chef_handler opscode cookbook](https://github.com/opscode/cookbooks/tree/master/chef_handler).
The handler submits passive checks to nagios via nsca, originally forked from [ranjibd](https://github.com/ranjibd/nsca_handler)


Attributes
==========

`default["chef_handler"]["handler_path"]` - location to drop off handlers directory, default is `/var/chef/handlers`.
`default["chef_handler"]["nsca_server"]` - ip  or hostname of the nsca server.
`default["chef_handler"]["nsca_port"]` - custom port of the nsca server.


# Example


###recipe

        #Place the handlers script

       Chef::Log.info("Chef Handlers will be at: #{node['chef_handler']['handler_path']}")

       remote_directory node['chef_handler']['handler_path'] do
         source 'handlers'
         owner 'root'
         group 'root'
         mode "0755"
         recursive true
         action :nothing
       end.run_action(:create)


       chef_handler "MyOrganization::NSCAHandler" do
         source "#{node['chef_handler']['handler_path']}/nsca_handler"
         supports :report => true, :exception => true
         action :enable
       end
