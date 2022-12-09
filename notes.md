# Notes

- log files
  -  /var/log/pods
  -  /var/log/containers
  -  journalctl -fu <service>
  - /var/log/syslog
  - crictl logs
- existing labels won't be allowed to modified once NodeRestriction is enabled using --enable-admission-plugins=NodeRestriction
