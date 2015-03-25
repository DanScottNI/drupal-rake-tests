require 'sshkit'
require 'sshkit/dsl'

server                = ENV['dru_server'] || "sveapp01.thestables.net"
ssh_user              = ENV['dru_ssh_user'] || "scottd" # for rsync deployment
site_root             = ENV['dru_site_root'] || "/home/dan/Documents/git/drupal/nicorporatewebsites/drupal"
remote_root           = ENV['dru_remote_root'] || "/opt/rh/httpd24/root/var/www/html/jenkins/" # for rsync deployment
dbuser                = ENV['dru_dbuser'] || "testuser"
dbpassword            = ENV['dru_dbpassword'] || ""
db                    = ENV['dru_db'] || ""
dbserver              = ENV['dru_dbserver'] || ""
dbserverport          = ENV['dru_dbserverport'] || ""
drupal_profile        = ENV['dru_drupal_profile'] || "nics"

task :build do |t, args|
  puts "Current env is #{ENV['RAKE_ENV']}"
end

desc "Rsync the files from the drupal directory to the remote host"
task :rsync_files do
  system "rsync -avz --exclude "".git"" --exclude "".gitignore"" --progress #{site_root} #{ssh_user}@#{server}:#{remote_root}"
end

task :drush_set_maintenance_mode_on do
  on "#{ssh_user}@#{server}" do |host|
    within "#{remote_root}" do
      execute :drush, 'vset maintenance_mode 1 -y'
    end
  end
end

task :drush_set_maintenance_mode_off do
  on "#{ssh_user}@#{server}" do |host|
    within "#{remote_root}" do
      execute :drush, 'vset maintenance_mode 0 -y'
    end
  end
end

desc "Deploy the site, pulls from Git, migrate the db and precompile assets, then restart Passenger."
task :drush_site_install do
  on "#{ssh_user}@#{server}" do |host|
    within "#{remote_root}" do
      execute :mkdir, 'danstest'
    end
  end
end
