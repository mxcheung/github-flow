git for-each-ref --sort=committerdate refs/heads/ \
  --format='%(refname:short) | %(committerdate:iso8601)' \
  | awk -F'|' '$2 < strftime("%Y-%m-%d", systime() - 365*24*3600)'