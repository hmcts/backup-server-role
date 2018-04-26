Role Name
=========

Ansible role for setting up backups using rsnapshot

Requirements
------------

Configures rsnapshot backups and schedules cronjobs


Role Variables
--------------

# SSH key for the 'root' user, the public one should be set on remote
# servers to backup (see the role 'rsnapshot-slave')
rsnapshot_ssh_key: id_rsa
rsnapshot_user: root
# rsnapshot options
rsnapshot_config_file: /etc/rsnapshot.conf
rsnapshot_config_version: 1.2
rsnapshot_config_snapshot_root: /var/cache/rsnapshot/
rsnapshot_config_no_create_root: 1
rsnapshot_config_cmd_cp: /bin/cp
rsnapshot_config_cmd_rm: /bin/rm
rsnapshot_config_cmd_rsync: /usr/bin/rsync
rsnapshot_config_cmd_ssh: /usr/bin/ssh
rsnapshot_config_cmd_logger: /usr/bin/logger
rsnapshot_config_cmd_du: False
rsnapshot_config_cmd_rsnapshot_diff: False
rsnapshot_config_cmd_preexec: False
rsnapshot_config_cmd_postexec: False
rsnapshot_config_intervals:
    - name: hourly
      value: 6
    - name: daily
      value: 7
    - name: weekly
      value: 4
    - name: monthly
      value: 3
rsnapshot_config_verbose: 2
rsnapshot_config_loglevel: 3
rsnapshot_config_logfile: /var/log/rsnapshot.log
rsnapshot_config_lockfile: /var/run/rsnapshot.pid
rsnapshot_config_stop_on_stale_lockfile: 0
rsnapshot_config_rsync_short_args: False
rsnapshot_config_rsync_long_args: '--delete --numeric-ids --relative --delete-excluded --rsync-path=rsync-wrapper.sh'
rsnapshot_config_ssh_args: '-i /root/.ssh/{{ rsnapshot_ssh_key }}'
rsnapshot_config_du_args: False
rsnapshot_config_one_fs: False
rsnapshot_config_include: []
rsnapshot_config_exclude: []
rsnapshot_config_include_file: False
rsnapshot_config_exclude_file: False
rsnapshot_config_link_dest: 1
rsnapshot_config_sync_first: 0
rsnapshot_config_use_lazy_deletes: 0
rsnapshot_config_rsync_numtries: 0
rsnapshot_config_backup:
    - name: LOCALHOST
      points:
          - [backup, /home/, localhost/]
          - [backup, /etc/, localhost/]
          - [backup, /usr/local/, localhost/]

# rsnapshot crontab
rsnapshot_crontab_env: {}
rsnapshot_crontab:
    - name: hourly
      month: '*'
      weekday: '*'
      day: '*'
      hour: '*/4'
      minute: 0
      job: "/usr/bin/rsnapshot hourly"
    - name: daily
      month: '*'
      weekday: '*'
      day: '*'
      hour: 3
      minute: 30
      job: "/usr/bin/rsnapshot daily"
    - name: weekly
      month: '*'
      weekday: 1
      day: '*'
      hour: 3
      minute: 0
      job: "/usr/bin/rsnapshot weekly"
    - name: monthly
      month: '*'
      weekday: '*'
      day: 1
      hour: 2
      minute: 30
      job: "/usr/bin/rsnapshot monthly"


Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------
MIT

Author Information
------------------
HMCTS Reform Programme
