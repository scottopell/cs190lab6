cs190lab6
=========

Before starting this lab, we again urge you to check over the git reference sheet [Git Reference](./git_reference.md)

And the slides can be found here [Git week 2](https://docs.google.com/a/scottopell.com/presentation/d/1zmM8ySifYmx3arxsvLsEK9eUrGzWNsZdq1qGN8esA5E/)

Last weeks lab can be found [here](http://github.com/scottopell/cs190lab5) if you need to reference it for anything.

This is another partner lab, so buddy up!

## What You'll Learn
By the end of this lab, you'll be able to creat, use, merge and delete (local) branches.

## Setup ##

Depending on your working environment, perform the appropriate action:

| Environment   | Action        |
| ------------- | ------------- |
| Linux Lab Machine            | Open a terminal window        |
| Windows or Personal Computer | SSH into `data.cs.purdue.edu`*  |

----

\* You must have X11 forwarding enabled. If you are on the Windows Lab Computer, follow the instructions below:

1. Search for 'Xming' in the start menu and run it.
2. Open PuTTy
3. Expand the 'SSH' tab from the 'Category' list
4. Choose 'X11' from 'SSH' list
5. Check 'Enable X11 Forwarding'
6. Connect like normal to `data.cs.purdue.edu` within PuTTy.

### Git Setup

You should have your git username and email set up from last week, if you get any messages about missing them, refer to last weeks lab.


   ```bash
   git config --global push.default simple
   ```
 
### Setting up your repository. 
#### Do WITH your partner on ONE computer

1. Create a new repository on github and add your partner as a collaborator
1. Make and go into a directory for this lab. The path for me would be `~/sopell/cs190lab6`

  > Use `mkdir` and `cd` for this

2. Download `text_mutator.rb` into this directory.

  ```bash
  wget https://github.com/scottopell/cs190lab6/raw/master/text_mutator.rb
  ```
 
3. Initialize a repo, add, commit, and push this file.

  ```bash
  git init
  git add text_mutator.rb 
  git commit -m 'adds text_mutator.rb'
  git remote add origin <github_url>
  git push -u origin master
  ```
 
4. Have your partner clone this repository onto their machine.
5. Decide which one of you wants to debug, and which wants to add a new feature. Procede to either "Bug Fixes" or "New Features" depending on which one you're doing.

### Bug Fixes

Before we get started on the bug fix, we don't know how many commits or how long this will be, so lets make our own branch.

  ```bash
  git branch bug-fixes
  git checkout bug-fixes
  ```

So, you have your repository with a fresh version of `text_mutator.rb` If you run it, like so

  ```bash
  ruby text_mutator.rb --reverse Hello World
  ```

You'll notice that you get an output that's reversed, but it only reverses the first word. This isn't very much fun. Lets fix it.

1. Open up `text_mutator.rb` in your editor of choice and look through the code.
2. You can see that to get the input text (from line 12)

  ```ruby
  text = ARGV[1]
  ```

3. So that line just gets the second(because of 0-based indexing) argument after `text_mutator.rb`
4. Instead of that, lets prompt the user for input.

  > Since you guys (probably) don't know ruby, we'll give you most of the code

5. So instead of getting the data from text from the command line arguments, we can use `gets.chomp()`

  > `gets` stands for get string
  
  > `chomp()` gets rid of the trailing newline (don't worry about this for now)

6. So line 12 should now be

  ```ruby
  text = gets.chomps()
  ```

7. We should probably tell the user whats happening, so add a line above this that says

  ```ruby
  puts "Input some text, please"
  ```

8. Now save this, exit, and run the program again.

  ```bash
  ruby text_mutator.rb --reverse
  ```

9. Now we should be prompted for input, so type in some text and it should output it as expected!
10. Lets `add` our changes

  ```bash
  git add text_mutator.rb
  ```
 
11. `commit` these changes

  ```bash
  git commit -m 'fixes bug with text input, changes source from argv to user input'
  ```
 
12. Now lets push our changes to the remote 

  ```bash
  git push -u origin bug-fixes
  ```
 
  > Note, its our first push on this branch, so we want to use `-u`, and we're pushing to the remote named `origin` and our branch is named `bug-fixes` 

13. So we can log onto github and see our branch!
14. And we like our changes, so lets merge them back into master

  ```bash
  git checkout master
  git merge bug-fixes
  ```

15. And lets push our changes up to the remote

  ```bash
  git push
  ```
 
16. Now lets wait for our partner to finish up and continue on to "Wrap-up"

### New Features

While your partner is fixing bugs, we're going to add some new features to this program!

If you run it with no arguments, like this

  ```bash
  ruby text_mutator.rb
  ```

We'll see a message showing us how to use the program.

  ```
  #example output
  Usage: ruby text_mutator.rb [--reverse --upper --lower --upsidedown] <input text>
  ```

So lets run it with the following input

  ```bash
  ruby text_mutator.rb --upper scott
  ```

And we'll see our output

  ```bash
  ttocs
  ```

Lets add some new options.

We'll need to make modifications in the switch case (lines 15 through 28)

and we'll need to add our own modification method (lines 36 through 50)

We want to add an option to to invert the case of the string, so our goal is to be able to do the following

  ```bash
  ruby text_mutator.rb --swapcase heLlO
    >>HElLo
  ```

First thing we need to change is the switch case

1. Add these lines under line 23, in the same format as the ones above, to add support for the `--swapcase` flag

  ```ruby
  when "--swapcase"
    mod_text = swapcase_mod text
  ```

2. Now that we've added a call to the method `swapcase_mod`, we need to create that method. We can do this by inserting these lines under line 51.

  ```ruby
  def swapcase_mod input
    input.swapcase
  end
  ```

3. That's it! Now we can save this, exit, and try running it with our new method like this.

  ```bash
  ruby text_mutator.rb --swapcase hElLo
  ```

  And hopefully we'll see this output

  ```
  HeLlo
  ```

Now just wait for your partner to finish up theres and then continue on to "Wrap-up"
