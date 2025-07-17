name: Update README

on:
  schedule:
    - cron: '0 * * * *'  # Updates every hour
  workflow_dispatch:

jobs:
  update-readme:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Update README
        run: |
          GREETING=$(date +"%H" | awk '{if ($1 < 12) print "Good Morning"; else if ($1 < 18) print "Good Afternoon"; else print "Good Evening"}')
          DATE=$(date +"%A, %B %d, %Y")
          TIME=$(date +"%I:%M:%S %p")
          
          cat > README.md << EOF
          # $GREETING
          **$DATE**  
          *$TIME*
          EOF
          
      - name: Commit changes
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add README.md
          git commit -m "Update greeting and time" || exit 0
          git push
