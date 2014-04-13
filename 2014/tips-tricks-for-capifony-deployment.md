In this blog post we want to share some tips & tricks for deploying with [capifony](http://capifony.org)
which you might find useful as well.

## Upload parameters files per server

Capifony already supports the upload of a *parameters.yml* file to servers during the deployment.
This is done globally or for each stage separately, what is already
[documented as a cookbook](http://capifony.org/cookbook/upload-parameters-file.html). The parameters files
don't need to be in the repository, they just have to be on the machine where you run the deployment.

In our project each server requires its own license key for an external service. For that reason we
need a separate *parameters.yml* file for each server and can't use the stages to distinguish them.
To solve this problem we created a capifony task which can upload a different parameters file to each server.

First, we define the separate files for the servers.

    # app/config/deploy/prod.rb
    server 'server1.example.com', :app, :web, :primary => true, :parameters_file => 'production1.yml'
    server 'server2.example.com', :app, :web, :primary => true, :parameters_file => 'production2.yml'

The directory of the files is set as a variable.

    # app/config/deploy.rb
    set :parameters_dir, "app/config/parameters"

Now comes the main part, we create a new task for uploading the file.

    # app/config/deploy.rb
    task :upload_parameters, :except => { :parameters_file => nil } do
      servers = find_servers_for_task(current_task)
      servers.each do |server|
        parameters_file = server.options[:parameters_file]

        origin_file = parameters_dir + "/" + parameters_file if parameters_dir && parameters_file
        if origin_file && File.exists?(origin_file)
          ext = File.extname(parameters_file)
          relative_path = "app/config/parameters" + ext

          if shared_files && shared_files.include?(relative_path)
            destination_file = shared_path + "/" + relative_path
          else
            destination_file = latest_release + "/" + relative_path
          end

          run "#{try_sudo} mkdir -p #{File.dirname(destination_file)}", :hosts => server

          capifony_pretty_print "--> Uploading " + parameters_file + " to " + server.host
          top.upload(origin_file, destination_file, { :hosts => server })
          capifony_puts_ok
        end
      end
    end

This task is very similar to the one of the cookbook. We use the *:except* option to run this
task only for servers which have the *parameters_file* property defined.

Then you can run the task.

- For a shared parameters file: after 'deploy:setup', 'upload_parameters'
- For an unshared parameters file: before 'deploy:share_childs', 'upload_parameters'

## Generate parameters files per server

Instead of uploading the parameter files they can also be generated during the deployment. This can
be used if you don't want to copy around files and just want to import another parameters file inside
the *parameters.yml*. For this a slightly different task is needed.

    # app/config/deploy.rb
    task :generate_parameters, :except => { :parameters_file => nil } do
      servers = find_servers_for_task(current_task)
      servers.each do |server|
        parameters_file = server.options[:parameters_file]
        ext = File.extname(parameters_file)
        relative_path = "app/config/parameters" + ext

        if shared_files && shared_files.include?(relative_path)
          destination_file = shared_path + "/" + relative_path
        else
          destination_file = latest_release + "/" + relative_path
        end

        run "#{try_sudo} mkdir -p #{File.dirname(destination_file)}", :hosts => server

        capifony_pretty_print "--> Generating parameters file on " + server.host
        run "#{try_sudo} echo -e \"imports:\\n    - { resource: parameters/#{parameters_file} }\" >#{destination_file}", :hosts => server
        capifony_puts_ok
      end
    end

The rest remains the same as for uploading the parameters files.

With this solution, the *parameters.yml* files have to be committed to the repository. We decided to do
this because it makes the maintenance easier, e.g. we see the changes directly in our merge/pull requests.

## Update schema with multiple entity managers

If you need an entity manager for updating the schema, which is not the default one, you can set a variable.
All doctrine tasks in capifony will use this variable.

    set :doctrine_em, 'custom_em'

But if you have multiple entity managers and want to update the schema for all of them, a little more
work is required.

    # Default entity manager
    after 'deploy:create_symlink', 'symfony:doctrine:schema:update'
    
    # Custom entity manager 1
    after 'deploy:create_symlink' do
      set :doctrine_em, 'custom_em_1'
      symfony.doctrine.schema.update
      set :doctrine_em, false
    end
    
    # Custom entity manager 2
    after 'deploy:create_symlink' do
      set :doctrine_em, 'custom_em_2'
      symfony.doctrine.schema.update
      set :doctrine_em, false
    end

The variable should be set to the default value again after running the task to avoid conflicts with other
existing tasks.

## Add options when running custom Symfony commands

If you run some custom Symfony commands during deployment don't forget to add the *console_options*
variable. This variable defines the *--env* and *--no-debug* parameters.

    run "#{try_sudo} sh -c 'cd #{latest_release} && #{php_bin} #{symfony_console} custom:command #{console_options}'"

Commands without that will run in the *dev* environment what you probably don't want.

