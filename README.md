proxysql
=========

Currently this role is compatible with ProxySQL v2

Requirements
------------

Ansible 2.5 minimum

Role Variables
--------------

The variables that can be passed to this role. For all variables, take
a look at [defaults/main.yml](defaults/main.yml).

```yaml
# defaults file for proxySQL
proxysql_cnf_path: '/etc'
proxysql_template: 'proxysql.cnf.j2'

# admin variables
proxysql_admin_credentials: 'admin:admin'
proxysql_admin_ifaces: '0.0.0.0:6032'
proxysql_admin_refresh_interval: 2000

# mysql variables
proxysql_mysql_threads: 4
proxysql_mysql_max_connections: 2048
proxysql_mysql_default_query_delay: 0
proxysql_mysql_default_query_timeout: 36000000
proxysql_mysql_have_compress: true
proxysql_mysql_poll_timeout: 2000
proxysql_mysql_interfaces: '0.0.0.0:6033' #0.0.0.0:6033;/tmp/proxysql.sock 
proxysql_mysql_default_schema: 'information_schema'
proxysql_mysql_stacksize: 1048576 
proxysql_mysql_server_version: '5.5.30'
proxysql_mysql_connect_timeout_server: 3000
proxysql_mysql_monitor_username: 'monitor'
proxysql_mysql_monitor_password: 'monitor'
proxysql_mysql_monitor_history: 600000
proxysql_mysql_monitor_connect_interval: 60000
proxysql_mysql_monitor_ping_interval: 10000
proxysql_mysql_monitor_read_only_interval: 1500
proxysql_mysql_monitor_read_only_timeout: 500
proxysql_mysql_ping_interval_server_msec: 120000
proxysql_mysql_ping_timeout_server: 500
proxysql_mysql_commands_stats: true
proxysql_mysql_sessions_sort: true
proxysql_mysql_connect_retries_on_failure: 10

# define all MySQL servers
proxysql_mysql_servers: []
  # - address: "127.0.0.1" # no default, required . If port is 0 , address is interpred as a Unix Socket Domain
  #	  port: 3306           # no default, required . If port is 0 , address is interpred as a Unix Socket Domain
  #	  hostgroup: 0	        # no default, required
  #	  status: "ONLINE"     # default: ONLINE
  #	  weight: 1            # default: 1
  #	  compression: 0       # default: 0
  #   max_replication_lag: 10  # default 0 . If greater than 0 and replication lag passes such threshold, the server is shunned
  #   max_connections: 200

# defines all the MySQL users
proxysql_mysql_users: []
  # - username: "username" # no default , required
  #   password: "password" # default: ''
  #		default_hostgroup: 0 # default: 0
  #		active: 1            # default: 1
  #		max_connections: 1000
  #		default_schema: "test"

# defines MySQL Query Rules
proxysql_mysql_query_rules: []
  # -	rule_id: 1
  #		active: 1
  #		match_pattern: "^SELECT .* FOR UPDATE$"
  #		destination_hostgroup: 0
  #		apply: 1
  
  #	-	rule_id: 2
  #		active: 1
  #		match_pattern: "^SELECT"
  #		destination_hostgroup: 1
  #		apply: 1


proxysql_scheduler: []
  # - id: 1
  #   active: 0
  #   interval_ms: 10000
  #   filename: "/var/lib/proxysql/proxysql_galera_checker.sh"
  #   args:
  #     - "0"
  #     - "1"
  #     - "0"
  #     - "1"
  #     - "/var/lib/proxysql/proxysql_galera_checker.log"

proxysql_mysql_replication_hostgroups: []
# - writer_hostgroup: 30
#   reader_hostgroup: 40
#   comment: "test repl 1"
#                
# - writer_hostgroup: 50
#   reader_hostgroup: 60
#   comment: "test repl 2"

proxysql_mysql_galera_hostgroups: []
# - writer_hostgroup: 60
#   backup_writer_hostgroup: 61
#   reader_hostgroup: 70
#   offline_hostgroup: 79
#   health_check_enabled: True
#   health_check_port: 9200
#   health_check_type: galera
#   comment: "Galera cluster 1"
#                
# - writer_hostgroup: 80
#   backup_writer_hostgroup: 81
#   reader_hostgroup: 90
#   offline_hostgroup: 99
#   comment: "Galera cluster 2"
#
# NOTE: Add your servers in the reader hostgroup, ProxySQL will take care of everything else.
```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - proxysql


Health check
----------------
This role installs a HTTP health check for galera host groups. By default this runs on port 9200, if you configure multiple galera hostgroups with this role you need to specify a different port.
Contributions to install health checks for other hostgroups are appreciated.

License
-------

BSD

Author Information
------------------

Julien Bunel (julien@laratuts.fr)
