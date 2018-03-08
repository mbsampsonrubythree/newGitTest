Create a Repository
Click the "+" icon at the to right of GitHub's page
Assign a name and desc for your repo
"Create new Repository"

Configure Git and Rails
Open the Terminal
Change Dir to your rails app folder
Initialize an empty Git repo
git init
View Files to be Committed
git status   (red items here have yet to be committed)  
Commit Item/Items
git add  <file_name>
git add .    //   git add -all
Confirm and Name your Commit
git commit -m '<name your commit>'
NOTE: Use a concise commit name in case you need to rollback to a previous point
Push Changes to Repository
First Time
Log onto GitHub and find your repositry
Navigate to the 'Code' tab
...or push an existing repository from the command line
Copy the contents of this section into your Terminal
Regular Pushes
git push
Show Past Commits
git log
Remove Changes
git checkout <file_to_revert>

GitIgnore - How and Why
    During the development of your application you're going to have files that you don't want uploaded to a public place. The first file that comes to mind is the secrets.yml file that resides in all rails apps. This file holds encryption code keys for your data -- very important these be!

Ignore a File with .gitignore
Navigate to your app's dir and open the .gitignore file
You'll have to use open .gitignore since the file is hidden by default
 Add the file path of the file that you want to ignore and provide a comment:  /config/secrets.yml
Changes made to secrets.yml will still push through - to avoid this...
git rm . -r --cached
git status
git add .
git commit -m "cleared git cache"
git push
Now your file should be omitted when you make pushes.



Creating a New Application

Change Dir to the folder that you want the application to reside in
Create a Rails App
rails new APP_NAME
Change Dir into the app's folder and install the bundle
bundle install
Test the App
rails s
navigate to localhost to see if it works
Create a DB
rails db:create && rails db:migrate
Create a Scaffold
rails generate scaffold MODEL_NAME   ATTRIBUTE:DATATYPE   ATTRIBUTE DATATYPE
Migrate DB
rails db:migrate
Test the App
rails s
Test


Rails Environment

/Assets
    This is where your SCSS, js, images, and config files are located.

/Controllers
    Controllers provide a map for your application. They handling incoming requests and direct the browser to different routes depending on the user's request and CRUD (create, read, update, delete) standards.

    Let's say that you navigate to http://localhost:3000/blogs through your browser. The default action that a controller takes when you visit this URL is to send you to the controller's index (this is a read operation). Once you're on that page the controller renders the view for that page (app/views/blogs/index.html.erb) and displays itself to you.

  # GET /blogs
  # GET /blogs.json
  def index
    @blogs = Blog.all
    # @blogs is an instance variable and is passed into the view so that the
    # view can render the DB's data to display to the user
    # http://localhost:3000/blogs
  end

<!--
This is the view for http://localhost:3000/blogs
In the Ruby code below we take each Blogs DB item and display it using HTML.
 -->
<p id="notice"><%= notice %></p>

<h1>Blogs</h1>

<table>
  <thead>
    <tr>
      <th>Title</th>
      <th>Body</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% @blogs.each do |blog| %>
      ...

<%= link_to 'New Blog', new_blog_path %>

Before_action Explained
class BlogsController < ApplicationController
# Before anything happens in the controller add the contents of set_blog:
#     @blog = Blog.find(params[:id])
# to the show, edit, update, and destroy routes
  before_action :set_blog, only: [:show, :edit, :update, :destroy]
# This allows us to follow the DRY mentality which eliminates verbose code blocks.

  def index
    @blogs = Blog.all
  end

  def show
      # @blog = Blog.find(params[:id])
  end

  def new
    @blog = Blog.new
  end

  def edit
      # @blog = Blog.find(params[:id])
  end

  private
    def set_blog
      @blog = Blog.find(params[:id])
      # This variable is added to the show, edit, update, and destroy methods
    end
end

Controller Explained
def index
    @blogs = Blog.all
end

def show
end

def new
    # Blog.new doesn't create a new Blog. New tells controller to open the
    # the /views/blogs/new.html.erb and display that to the user. It also
    # creates a new Blog object.
    @blog = Blog.new
end

# [localhost/blogs/new] directs the user to the new.html.erb view
# The view provides a place for the user to input data for the Blog
# Upon submission the contents of the form tags are given to @blog
# if the blog can be saved (meets validation requirements) a success notice is shown
# else an error notice is posted to the user.

def edit
  # The edit path is linked to UPDATE
end

def create
  # Takes the blog_params that the user provided in the new.html.erb and stores
  # them as @blog.
  @blog = Blog.new(blog_params)

  respond_to do |format|
    if @blog.save
      # If @blog contains a valid blog (passes validation via the model) then
      # the notice HTML tag receives 'Blog was successfully created'
      # and the user is directed to the show.html.erb page.
      format.html { redirect_to @blog, notice: 'Blog was successfully created.' }
    else
      format.html { render :new }
      format.json { render json: @blog.errors, status: :unprocessable_entity }
    end
  end
end

An Object is Saved/Updated
# user visits localhost/blogs/new
# 1. The new.html.erb view is rendered
# 2. Upon 'Create Blog' the form data is saved into @blog via Blog.new(blog_params)
     def blog_params
       params.require(:blog).permit(:title, :body)
     end
     # Takes from <input id="blog_title"> and <textarea id="blog_body"> from new view
# 3. Create method is hit and receives this data from the new view
# 4. If the blog can be saved then the user receives a success notice OTHERWISE an
#    error notice is shown.       


CLI Notes

Commands
List all files                                       ls
List all files (+hidden)                    ls -a
List all files (+format)                    ls -la
Make Dir (folder)                            mkdir <folder name>
Change Dir                                        cd <target folder>
    Backtrack                                      cd ..
Current Dir                                        pwd
Create a file (in current dir)         touch <file>
Move File                                           mv <file name>  <target folder>
    Alter File Name                            mv <file name>  <new name>
Remove File                                      rm <file name>
    Remove Folder                             rm <folder name>
    (Note) rm -rf <folder name> OVERRIDES the rm folder precautions

Read (Show) File Content              cat <file name>
Set Content to File                          echo "content here" > <target file>
Append Content to File                  echo "append content" >> <target file>
Copy File                                            cp <file name>  <new file name>
Copy File                                            cp <file name> ~<target location/file name>
                                                             cp text.txt ~/Desktop/new_text.txt
 Show All System Routes               rake routes
