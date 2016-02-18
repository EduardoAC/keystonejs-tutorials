#TUTORIAL HOW TO INTEGRATE COMMENTS IN OUR KEYSTONEJS BLOG
This tutorial have the goal to introduce in 4 easy steps native comments to
our blog because what it's a blog without user posts.

However, this is a basic tutorial doesn't contain elements as comment moderation,
asynchronous comments load or cross site scripting. Check another tutorials to
increase the complexity of your code keeping secure and efficient.

Additionally, I want to thanks the creators of keystone-demo repository where
I extracted most of this code.

**NOTE: if you find any issue, bug or question please create either or issue.
All your support and comments are welcome**

## STEP 0: ENVIRONMENT SETUP (KEYSTONEJS INSTALLATION, SKIP IF YOU DID ALREADY)

This step is a prerequisite if you are starting from scratch and it will be useful
if you decide to use any other configuration options to contextualize how this tutorial
is built.

For this tutorial I decided to use the next configuration when you execute [getting started installation using Yeoman] (http://keystonejs.com/getting-started/). The answer are the ones you need to pick

- What is the name of your project? (My Site) My comments tutorial
- Would you like to use Jade, Swig, Nunjucks or Handlebars for templates? jade
- Which CSS pre-processor would you like? [less | sass | stylus] (less) sass
- Would you like to include a Blog? (Y/n) Y
- Would you like to include an Image Gallery? (Y/n) n
- Would you like to include a Contact Form? (Y/n) n
- What would you like to call the User model? (User) Y
- Enter an email address for the first Admin user: user@keystonejs.com
- Enter a password for the first Admin user: admin
- Would you like to include gulp or grunt? [gulp | grunt] grunt
- Would you like to create a new directory for your project? Yes

After you answered these easy question, you will be able to execute the command
'node keystone'
**REMEMBER your mongod service have to be up and running**

##STEP 1: DEFINE COMMENT MODEL AND RELATIONSHIPS

Once we get our environment working. It's time to start to build our native comment
system. Therefore, we need to create our model that I will call PostComment to reference
the keystone-demo example.

As all of models in keystone they have a specific structure that I won't explain in this
tutorial to manage MongoDB and KeystoneJS.

However, I want to point three important elements that you must not forget when
you are creating your comment model for your site.

1. .wasActive() function doesn't come in the User base model so you will have to add it
as you can see for the User model in the commit [STEP 1](https://github.com/EduardoAC/keystonejs-tutorials/blob/14125f18e957f17eb5aca8766c54a238c6785480/my-comments-tutorial/models/User.js)
2. You have to create a relation 1-M between the Post and PostComment models, [here the code](https://github.com/EduardoAC/keystonejs-tutorials/blob/14125f18e957f17eb5aca8766c54a238c6785480/my-comments-tutorial/models/Post.js).
3. You need to sanitize the content before save in the database (it's omitted in this to tutorial for simplicity).

Finally, I would like to encourage you to create Unit test to help you avoid
errors, an automated battery of tests can save you a lot to time in the future.

##STEP 2: ADD COMMENTS TO ADMIN UI

This is a really easy task in KeystoneJS. Add 'post-comments' to your navigation
bar as you can see in [STEP 2 commit](https://github.com/EduardoAC/keystonejs-tutorials/commit/71dba619d8a482edd702d83d6f45f3a7e2b3e16c) and you are ready to create comments for your current users and posts.

**REMEMBER: RESTART NODE KEYSTONE TO SEE THE NEW CONFIGURATION"
**NOTE: THIS STEP IS SUBJECT TO CHANGE SOON WITH THE NEW REACT RELEASE OF THE ADMIN UI, I WILL KEEP YOU POSTED**

##STEP 3: CONFIG POST ROUTE TO ALLOW COMMENTS IN THE CLIENT UI.

In this step we need to keep in mind that we will create a POST & GET requests
to create and delete comments from the client UI so we have to enable it in the index file
as you can see in [here](https://github.com/EduardoAC/keystonejs-tutorials/blob/5faa2b66caa6b20719a071cb59ec8767b3c7969c/my-comments-tutorial/routes/index.js) where we change from only accept GET request to accept all kind of request.

Once we have this, we proceed to create three functions:

1. Retrieve comments to provide data to the Post view
2. Process POST request for new comments creation
3. Process GET request to allow delete comments.

**REMEMBER: We need to sanitize and do comment management, this code allow
anyone to delete any comment also if you don't own it**

##STEP 4: UPGRADE POST VIEW TO ALLOW COMMENTS

This part of here is based in the keystone-demo example on github. However, you
shouldn't take from there because the current version of KeystoneJS create a different
structure using locals.data (this tutorial) variables instead of locals (keystone-demo).

After I clarified why the code is similar, it's time to explain what [STEP 4 commit](https://github.com/EduardoAC/keystonejs-tutorials/commit/d2d1e46484c2f5af5c10afc88969235a31b3abbc)
contains and why we do in these way.

Firstly, we will explain the "templates/views/posts.jade". This file contains
the code to render the post content that means categories, date, author, etc.
Then it added here our magic code to create and render the comments coming from
the post router using mixins.

- comment-form(): will create a comment form to submit the content via POST request
- comment-thread(): will render all the comments stored in the post-comments

As you can see in commenting.jade contains both functions where we can easily see
that the form it's contains only one field that is the content and a basic logic
that verify we are signed in before we are be able to post a comment.

Secondly we see comment-thread that iterate through all comments rendering our
comment-post template.

## Conclusion
I really enjoyed doing this tutorial and you may find improvements so all your
comments are welcome, help me to do this tutorial even better.

Thanks again to all the keystone-demo contributors and KeystoneJS team that
inspire me to contribute in my own way to do this platform even easier to customize.
