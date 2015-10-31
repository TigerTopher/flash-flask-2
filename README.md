# flash-flask-2: flask tutorial part 2

```
date = "2015-30-10T02:23:05+08:00"
title = "Learn Flask Part 2"
authors = ["toph", "carl"]
```

Last tutorial we learned about the very basics of Flask. This time we will make a login page for our users, and initialize a database.

```
Guide Outline:
Foreword
I. Installing Flask
  A. Linux Guide
  B. Windows Guide  
II. Hello World in Flask
III. Templates
  A. Control Statements in Templates
  B. Loops in Templates
  C. Template Inheritance
```

## Web Forms

We will be using the extension Flask-WTF to handle web forms. Before we can use it, we'll have to make a configuration file in the root folder (file config.py):

```
WTF_CSRF_ENABLED = True
SECRET_KEY = 'you-will-never-guess'
```

Now we need the app to read and use the config file, by adding one line within app/__init__.py:

```
from flask import Flask

app = Flask(__name__)
app.config.from_object('config')

from app import views
```

### User Login Form

To create a form, we will make a class LoginForm which will contain the input fields (username and password). Here is the code for the form (file app/forms.py):

```
from flask.ext.wtf import Form
from wtforms import StringField, PasswordField, validators

class LoginForm(Form):
    username = StringField(u'Username', [validators.required(), validators.length(max=32, min=6)])
    password = PasswordField(u'Password', [validators.required(), validators.length(max=32, min=6)])
```

Notice that `username` is a `StringField` but `password` is a `PasswordField` since we want the password to be displayed as black circles while it's being typed.

### Form template

We will now create a standard-looking template which will have a username, password, and submit field. (file app/templates/login.html)

```
{% extends "base.html" %}
{% block content %}
    <h1>Sign In</h1>
    <form action="" method="post" name="login">
        {{ form.hidden_tag() }}
        <p>
            Username<br>
            {{ form.username(size=32) }}<br>
            {% for error in form.username.errors %}
                <span style="color: red;">[{{ error }}]</span>
            {% endfor %}<br>
        </p>
        <p>
            Password<br>
            {{ form.password(size=32) }}<br>
            {% for error in form.password.errors %}
                <span style="color: red;">[{{ error }}]</span>
            {% endfor %}<br>
        </p>
        <p><input type="submit" value="Sign In"></p>
    </form>
{% endblock %}
```

Note that we can also catch and print input errors using a for loop.


### Form Views

(insert description here) (file app/views.py)

```
from flask import render_template, flash, redirect
from app import app
from .forms import LoginForm

@app.route('/')
@app.route('/index')
def index():
    user = {'nickname': 'Juan Dela Cruz'}
    posts = [
                {
                    'author': {'nickname': 'John'},
                    'body': 'Lorem ipsum dolor sit amet'
                },
            ]
    return render_template('index.html', title='', user=user, posts=posts)

@app.route('/login', methods=['GET', 'POST'])
def login():
    form = LoginForm()
    if form.validate_on_submit():
        return redirect('/index')
    return render_template('login.html', title='Sign In', form=form)
```
(insert description here)


Now try running the web server then going to `localhost:5000/login`. Notice that if we enter a username or password that is less than 6 characters or more than 32 characters long, the form outputs an error. Else, it redirects to index, which means the output was valid.






## References
All thanks to Miguel Grinberg's [Mega-Tutorial Site](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).

