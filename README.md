# NSWI177 Lab 7: Basic web page generator for GitHub
# http://tinyurl.com/nswi177-lab7 

GitHub lets you easily host web pages. For example, if you are a user `tpetricek` and
have a repository `basic` that contains a branch `gh-pages` with some `.html` files
in it, GitHub will host those automatically at: https://tpetricek.github.io/basic/

Let's use this to create a simple web page generator from Markdown files!

## Setting up GitHub

1. Create an account at https://github.com (if you do not have one already)
2. Go to Settings - Add SSH key and add your public key from `~/.ssh` so that you can push easily
3. Create a new GitHub project (without initializing it)

## Create a new git repository

1. Somewhere on your machine, create a new repository using `git init`
2. Add remote using `git remote add git@<yourremote>.git` (using SSH)
3. Create a `gh-pages` branch and add an empty commit (use `--allow-empty` for `git commit`)
4. Push the branch to GitHub and switch back to `master`
5. Add some `.md` files with demo text (at least two, not called `index`)
6. Add `template.html` containing something like:
   ```
   <!DOCTYPE html>
   <html><body>
     $body$
   </body></html>
   ```
7. Add `.gitignore` file containing two lines: `.output/` and `index.md`

## Create a `build.sh` script

- Note, you should develop & test things in the terminal first and then copy to `build.sh`!
- Note, see the section below for various things you will need
- The `build.sh` script should do the following:

1. Generate `index.md` file based on the `.md` files in your directory containing something
   like (if the directory contains `hello.md` and `byebye.md`):
   ```
   - [hello](hello.html)
   - [byebye](byebye.html)
   ```
2.  Delete the `.output` directory and all its contents
3.  Clone the `gh-pages` branch of the current repository into `.output` directory
4.  Turn all `.md` files in the current directory into `.html` pages in the `.output` directory
    using `pandoc` and using your `template.html` template
5.  Navigate to `.output` and publish your changes using `git add`, `git commit`, `git push`
  (As a bonus, use a nice commit message based on today's date.)

## Commands you may find useful

- `ls *.md` to list all `.md` files in the current directory
- `sed 's/\.md//g'` to replace `.md` with empty string
- `awk '{ print "["$0"]("$0".html)" }'` to turn `foo` into `[foo](foo.html)`
- `git remote -v` with suitable `head` and `cut` to get the current repo's remote
- `VAR=$(some commands here)` to assing the result of a command to a variable
- `git clone --branch gh-pages [REPO] [DIR]` to clone `gh-pages` of `[REPO]` into `[DIR]`
- `for MD in *.md; do (...) done` to iterate over all `.md` files in current directory
- `FOO=${BAR%.*}.html` to replace `.md` extension with `.html` in a file in variable `BAR`
- `pandoc --template template.html file.md` to turn `file.md` into HTML using a template
- `date '+%Y-%m-%d %H:%M:%S` to get a current date and time
- `git push origin gh-pages` to push a branch `gh-pages` to remote
