# git-github-hands-on
git config --global user.email "knihaas-jpg"
git config --global user.email "you@knihaas.com"
git config --global init.computer

mkdir git-practice && cd git-practice
git init
echo "# Git Practice" > README.md
git add README.md
git commit -m "Add README"

printf "Line 1\nLine 2\nLine 3\n" > notes.txt
git add notes.txt
git commit -m "Add notes file"

echo "Line 4" >> notes.txt
git add notes.txt
git commit -m "Add line 4 to notes"

git remote add origin https://github.com/<your-username>/git-practice.git
git branch -M main
git push -u origin main
