from datetime import datetime

readme_6mo = f"""# 🧹 Git – List Branches Older Than 6 Months

This guide shows how to find **Git branches whose last commit is older than 6 months**, both locally and remotely.  
You can list them with or without dates.

---

## 🧭 List Local Branches Older Than 6 Months (with date)

```bash
git for-each-ref --sort=committerdate refs/heads/ \
  --format='%(refname:short) | %(committerdate:iso8601)' \
  | awk -F'|' '$2 < strftime("%Y-%m-%d", systime() - 180*24*3600)'
```

**Example output:**
```
feature/old-report | 2023-09-15 10:14:03 +0000
bugfix/legacy-api  | 2023-04-07 09:44:55 +0000
```

---

## 🧾 List Local Branches Older Than 6 Months (name only)

```bash
git for-each-ref --sort=committerdate refs/heads/ \
  --format='%(refname:short) %(committerdate:iso8601)' \
  | awk '$2 < strftime("%Y-%m-%d", systime() - 180*24*3600) {{print $1}}'
```

**Example output:**
```
feature/old-report
bugfix/legacy-api
```

---

## 🌐 List Remote Branches Older Than 6 Months (with date)

```bash
git fetch -p  # cleans up stale remote refs
git for-each-ref --sort=committerdate refs/remotes/origin/ \
  --format='%(refname:short) | %(committerdate:iso8601)' \
  | awk -F'|' '$2 < strftime("%Y-%m-%d", systime() - 180*24*3600)'
```

---

## 🌐 List Remote Branches Older Than 6 Months (name only)

```bash
git fetch -p
git for-each-ref --sort=committerdate refs/remotes/origin/ \
  --format='%(refname:short) %(committerdate:iso8601)' \
  | awk '$2 < strftime("%Y-%m-%d", systime() - 180*24*3600) {{print $1}}'
```

**Example output:**
```
origin/feature/legacy-cleanup
origin/bugfix/old-pipeline
```

---

## ⏳ Show Relative Age (e.g. “7 months ago”)

```bash
git for-each-ref --sort=committerdate refs/heads/ \
  --format='%(refname:short) | %(committerdate:relative)' \
  | grep -E 'month|year'
```

**Example output:**
```
feature/legacy-cleanup | 8 months ago
bugfix/deprecated-call | 1 year, 2 months ago
```

---

## ✅ Show Only Merged Branches Older Than 6 Months

```bash
git for-each-ref --sort=committerdate refs/heads/ \
  --format='%(refname:short) | %(committerdate:iso8601)' \
  | grep -Ff <(git branch --merged | sed 's/^[* ] //') \
  | awk -F'|' '$2 < strftime("%Y-%m-%d", systime() - 180*24*3600)'
```

Or **name-only** version:

```bash
git for-each-ref --sort=committerdate refs/heads/ \
  --format='%(refname:short) %(committerdate:iso8601)' \
  | grep -Ff <(git branch --merged | sed 's/^[* ] //') \
  | awk '$2 < strftime("%Y-%m-%d", systime() - 180*24*3600) {{print $1}}'
```

---

## 🧹 Optional Cleanup (Delete Old Local Branches)

Delete merged local branches older than 6 months:

```bash
for b in $(git branch --merged | grep -vE '^\*|main|master'); do
  last_commit=$(git log -1 --format="%ct" $b)
  age_days=$(( ( $(date +%s) - $last_commit ) / 86400 ))
  if [ $age_days -gt 180 ]; then
    echo "Deleting $b ($age_days days old)"
    git branch -d $b
  fi
done
```

---

## 🪶 Notes
- Adjust `180` to change the age threshold (in days).
- Works with Bash (macOS, Linux, or Git Bash on Windows).
- Add `--merged` or `--no-merged` to filter by merge status.

---

**Author:** Your Name  
**Last updated:** {datetime.now().strftime('%Y-%m-%d')}
"""

readme_path_6mo = "/mnt/data/git-branch-age-6months-readme.md"
with open(readme_path_6mo, "w") as f:
    f.write(readme_6mo)

readme_path_6mo