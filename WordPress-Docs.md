instructions: |
  #### Getting Started
  If you're new to WordPress, the [First Steps With
  WordPress](http://codex.wordpress.org/First_Steps_With_WordPress)
  documentation will step you through the process of logging into the admin
  panel, customizing your blog, and changing your theme.

  You can find the admin Username and Password listed as "WordPress Admin User"
  and "WordPress Admin Password", respectively.

  The [WordPress Lessons](http://codex.wordpress.org/WordPress_Lessons) cover a
  wide range of topics for users and designers.

  #### Accessing Your Deployment
  If you provided a domain name that is associated with your Rackspace Cloud
  account and chose to create DNS records, you should be able to navigate to
  the provided domain name in your browser. If DNS has not been configured yet,
  please refer to this
  [documentation](http://www.rackspace.com/knowledge_center/article/how-do-i-modify-my-hosts-file)
  on how to setup your hosts file to allow your browser to access your
  deployment via domain name. Please note: some applications like WordPress,
  Drupal, and Magento may not work properly unless accessed via domain name.
  DNS should point to the IP address of the Load Balancer.

  #### Migrating an Existing Site
  If you'd like to move your existing site to this deployment, there are
  plugins available such as
  [duplicator](http://wordpress.org/plugins/duplicator/) or [WP Migrate
  DB](http://wordpress.org/plugins/wp-migrate-db/) that can help ease the
  migration process.  This will give you a copy of the database that you can
  import into this deployment.  There are a number of other tools to help with
  this process.  Static content that is stored on the filesystem will need to
  be moved manually.  These files should be copied to the Master server, which
  will automatically synchronize the contents to all secondary servers.

  #### Plugins
  There are over 23,000 plugins that have been created by an engaged developer
  community. The [plugin directory](http://wordpress.org/extend/plugins/)
  provides an easy way to discover popular plugins that other users have found
  to be helpful. [Managing
  Plugins](https://codex.wordpress.org/Managing_Plugins) is important and you
  should be aware that some plugins decrease your site's performance so use
  them sparingly.  Please note that some plugins may require you to adjust the
  settings for PHP in order for them to function properly.  Please contact
  your Support team if you need assistance making these changes.

  #### Scaling out
  This deployment is configured to be able to scale out easily.  However,
  if you are expecting higher levels of traffic, please look into one of our
  larger-scale stacks.

  #### Details of Your Setup
  This deployment waws stood up using [Ansible](http://www.ansible.com/).
  Once the deployment is up, Ansible will not run again unless you update the
  stack.  Any changes made to the configuration may be overwritten when the stack
  is updated.

  WordPress itself was installed using [WP-CLI](http://wp-cli.org/). WordPress
  is installed in /var/www/vhosts/YOUR DOMAIN/ and served by [Apache](http://httpd.apache.org/).
  The Apache configuration file will be named using the domain name used as a
  part of this deployment (for example, domain.com.conf).

  Because this stack is intended for lower-traffic deployments, there is no
  caching configured.

  [Lsyncd](https://github.com/axkibe/lsyncd) has been installed in order to
  sync static content from the Master server to all secondary servers.
  When uploading content, it only needs to be uploaded to the Master node,
  and will be automatically synchronized to all secondary nodes.

  MySQL is being hosted on a Cloud Database instance, running MySQL 5.6.
  Backups for MySQL are provided by [Holland](http://wiki.hollandbackup.org/),
  which is running on the Master server.

  Backups are configured using Cloud Backups.  The Master server is configured
  to back up /var/spool/holland and /var/www once per week, and to retain
  these backups for 30 days.

  In order to restore the Database from backup, you will need to first restore
  /var/spool/holland from the appropraite backup.  After you have done so, you
  will need to log into the Master server, then run the following commands:

  gunzip /var/spool/holland/default/newest/backup_data/all_databases.sql.gz 
  mysql < /var/spool/holland/default/newest/backup_data/all_databases.sql

  Monitoring is configured to verify that Apache is running on both the Master
  and all secondary servers, as well as that the Cloud Load Balancer is
  functioning.  Additionally, the default CPU, RAM, and Filesystem checks
  are in place on all servers.

  #### Logging in via SSH
  The private key provided with this deployment can be used to log in as
  root via SSH. We have an article on how to use these keys with [Mac OS X and
  Linux](http://www.rackspace.com/knowledge_center/article/logging-in-with-a-ssh-private-key-on-linuxmac)
  as well as [Windows using
  PuTTY](http://www.rackspace.com/knowledge_center/article/logging-in-with-a-ssh-private-key-on-windows).
  This key can be used to log into all servers on this deployment.
  Additionally, passwordless authentication is configured from the Master
  server to all secondary servers.

  #### Additional Notes
  You can add additional servers to this deployment by updating the
  "server_count" parameter for this stack.  This deployment is
  intended for lower traffic or testing scenarios.
  
  This stack will not ensure that WordPress or the servers themselves are
  up-to-date.  You are responsible for ensuring that all software is
  updated.
