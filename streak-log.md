name: Daily Streak Maintainer

on:
  schedule:
    - cron: '0 4 * * *'  # Every day at 4 AM UTC
  workflow_dispatch:

jobs:
  update-streak:
    runs-on: ubuntu-latest
    env:
      GIT_AUTHOR_NAME: ZeroHack01
      GIT_AUTHOR_EMAIL: mongwoiching2080@gmail.com
      GIT_COMMITTER_NAME: ZeroHack01
      GIT_COMMITTER_EMAIL: mongwoiching2080@gmail.com

    steps:
      - uses: actions/checkout@v3

      - name: Update streak log
        run: |
          DATE=$(date '+%Y-%m-%d')
          TIME=$(date '+%H:%M:%S')

          # Check if the file is empty or doesn't exist
          if [ ! -s streak-log.md ]; then
            echo "# 📅 Commit Streak Log\n" > streak-log.md
            echo "Initialized on $DATE at $TIME" >> streak-log.md
          else
            echo "$DATE $TIME - Keeping the streak alive 🔥" >> streak-log.md
          fi

          git add streak-log.md

          # Only commit if there are changes
          if git diff --cached --quiet; then
            echo "No changes to commit."
          else
            git commit -m "✅ Streak update: $DATE"
            git push
          fi
