ON MAC

cd ~/Projects/ptgvn_staging   # or wherever you cloned it

# 1. See what changed
git status

# 2. Add the files you changed
git add path/to/file1 path/to/file2
# or add everything:
git add .

# 3. Commit your changes
git commit -m "Explain what I changed"

# 4. Push to GitHub (main branch)
git push origin main

Now GitHub is updated.

ON SERVER

cd ~/ptgvn.site      # your live repo folder
git status           # just to check
git pull origin main

