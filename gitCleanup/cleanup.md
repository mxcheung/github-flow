# 🧹 Git – List Branches Older Than 1 Year

This guide shows how to find **Git branches whose last commit is older than 1 year**, both locally and on remotes.  
Useful for cleaning up stale branches.

---

## 🧭 List Local Branches Older Than 1 Year

```bash
git for-each-ref --sort=committerdate refs/heads/ \
  --format='%(refname:short) | %(committerdate:iso8601)' \
  | awk -F'|' '$2 < strftime("%Y-%m-%d", systime() - 365*24*3600)'
  
  
  git fetch -p
git for-each-ref --sort=committerdate refs/remotes/origin/ \
  --format='%(refname:short) %(committerdate:iso8601)' \
  | awk '$2 < strftime("%Y-%m-%d", systime() - 365*24*3600) {print $1}'
  
  
  