# Ruby and Rails Tips

## New Rails Project Setup

## Deploying to Heroku

##Git Procedures
####Create a New Branch (or navigate to existing)
1) cd into main project folder
2) to create a new branch use: git checkout -b (branchname)
  -if you've already created your branch: git checkout (branchname)
3) make sure you have all the most current changes from master: git pull origin master
4) Now you can begin working in your branch
####To Commit New Changes to Master
1) git add .
2) git commit -m "Commit message"
3) git pull origin master
4) git checkout master
5) git merge (branchname)
6) git push origin master
7) git checkout (branchname)


## Rails-isms
  - Models are ruby classes that inherit from `ActiveRecord::Base`
  - Models are singular in rails `User`, `Student`, and `Todo`
  - Controllers are pluralized in rails `UsersController`, `StudentsController`, and `TodosController`
  - Database table names are plural in rails `users`, `students`, and `todos`
  - There is _(typically)_ one folder in views for each model you've created. It is the pluralized version of the model name and holds all the html/erb templates for that model's controller. For example: the `User` model will have an action index on the `UsersController` that will render the `users/index.html.erb` template.
## Ruby Gotchas (so many)

## Problem Solving

