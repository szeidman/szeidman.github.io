---
layout: post
title:  "Models, Views, & Controllers"
date:   2017-06-23 13:30:52 +0000
---


I built a model-view-controller site with big help from Sinatra and ActiveRecord. It's called Packlist. The app is designed for touring cyclists to organize what they will be carrying, how much it weighs, and where they will carry it.

Here is how it works:

## These are models and they're looking good

I made three model classes that inherit from ActiveRecord's database management. Accordingly, each model has an ActiveRecord migration to create items.

### Users

Users are those who wish to access the site. A User is created with a username and a password encrypted through the bcrypt gem. A User has many panniers and many items through panniers. I set the user class to validate the presence of a username, and the bcrypt "has_secure_password" method validates that a password was entered. (I found out the latter when crafting error messages to be displayed to users.)

### Panniers

Panniers are bags mounted to the racks of a touring bicycle. For purposes of the app, handlebar bags and rack bags also count as panniers. Panniers have a name and location (e.g., "Front Left" for the left side of the bike's front rack). They belong to a user and have many items. I set validations to make sure they have a name and location. The latter is a bit redundant, as discussed below in the Views section, because I used a dropdown menu to ensure one of six valid locations is always selected.

### Items

Lastly, items are anything someone would want to take along on a trip. They have a name and a weight. Items belong to a pannier. I included validations to make sure a name and numerical weight is entered and that an associated pannier is selected.

## The Controllersphere

### Application Controller

Each of the above models has a corresponding controller that inherits from the Applicaiton Controller, which includes configuration settings (such as enabling sessions) that are useful for all the other controllers. In addtion it processes a get request for the main page. Finally, it includes a couple helper methods to verify whether a user is logged in and whether items are being accessed by the current user.

### Users Controller

Seemingly the most basic of the three model-related controllers and the first one an actual user would encounter, I focused on this one largely after the other controllers were already in place. It allows users to create themselves via a signup page and to log back in after doing so.

As I had first tried out with the other controllers, to avoid error messages when creating a new user I used validation checks before running save commands. That way, before the application could save the invalid data and raise an error, the web page itself could essentially tell the user what the error would be and make the user try again.

To validate whether a login occurs, the controller searches for the username a given user enters, makes sure that username already exists, and then uses bcrypt's "authenticate" method on the password to see if it matches the stored password digest. If either is not true, the user sees an error and is asked to try again.

### Panniers Controller

Both the Application Controller and the Users controller gravitate towards the Panniers controller and views once a User is logged in. This controller uses a series of RESTful routes to Create, Read, Update and Delete panniers. It allows panniers to add a new item when created or while being edited. It also allows items to be added or removed from a given pannier.

The methods generally all check to make sure a user is logged in (using the helper method from the Application Controller) and is the appropriate user. Because panniers are assigned a User ID, the controller searches by that ID to return only those panniers created by the same user.

As described above, I used validation methods to evaluate whether the user entered bad data and used locals to customize the message that the user would see if they did. Because of the aforementioned dropdown menu that ensures a valid location is always selected, the only validation error a user could encounter would be if they neglected to enter the name. Still, in case future code changes that I still used code to allow iterations of multiple error messages. Since I did most of the initial legwork for this in Items (where there are indeed chances for multiple errors) I will go into further detail in that section.

### Items Controller

The Items controller is quite similar to the Panniers Controller as they both employ RESTful routes. Because Users have Items through Panniers, the controller similarly guards against a user seeing items besides those that the user created.

User errors are invoked by validation checks by using ".new" instead of ".save" and ".assign_attributes" instead of ".update" on an instance variable. Once .new or .assign_attributes is called, the ".valid?" method is called in an if statement: if the variable is valid, .save or .update will in fact be called.

If it is not valid it means that there will be error messages. Conveniently, these turn out to be generated by the validations used in the model classes (which themselves can be customized, though I generally liked the defaults).

In order to bring these messages to the user, I did the following:
1. Create a variable local_error and assign it to a hash containing the key "error_list" and value of an empty array.
2. Next, it iterates through the "full_messages" of each of the instance variable's errors and adds them to the error_list's array.
3. It then calls the appropriate .erb view, generally the same one the user just put bad data on, and assigns locals to it. Since locals takes a hash I can just put "local_error" after it. (In situations where I know there can only be one type of error, I skip step 2 and just type out the error_list key and the contents of its array.
4. From there, the .erb gets some code to display the error message. Each .erb that could appear following errors will check whether loclas include the "error_list" and, assuming there is one, iterates through each error message in error_list's array in order to display them to the user.

## Real life & postcard Views

### Layout

The app uses a layout for common HTML and embedded Ruby elements across pages, with a "yield" to pass in specific HTML from each view. This layout pulls in stylesheets and scripts to enable Bootstrap. I also incorporated a navbar from Bootstrap that changes depending on whether a user is logged in or out: once a user logs in, their username is displayed in the navbar along with tabs to view each of their panniers and items and to log out.

### Index

The main page for a user who is not logged in, this view just includes a simple slogan. Options to sign up or log in are in the navbar.

### Users

#### Create User

The "sign up" page. It first checks locals to see if there are any errors passed to it (e.g. if a user failed to enter a username and/or password). It then implements a form for users to enter a username and password they can submit to create an account.

#### Log in

After creating an account, a user is directed to the log in page. It also checks for errors, since the page will reload after a user enters invalid credentials or attempts to access a different URL while not logged in.

### Panniers

#### Index

After a user logs in, they are directed to the Panniers Index page, which iterates through all of the panniers created by that user and displays their name (with a link to the pannier's show page) and location. It also includes the option to add a pannier.

#### New

The New view also looks for errors from locals. It includes a form that takes in a pannier's name and requires selection of one of six pre-set locations. It then searches for any item in a pannier with the same user ID as the current user and presents them as checkboxes to add or remove existing items. Finally, it allows for the creation of a new item to be immediately placed into the new pannier before submitting.

#### Edit

Largely similar to the New view, but uses "patch" to allow updating. It pre-fills the name and location of an existing pannier and pre-checks any existing items, while preserving the option to create a new item.

#### Show

The page for an individual pannier. It displays the pannier's name and location and iterates through the pannier's items to display their name and weight in a table that also gives the total amount of weight in the pannier. (Pedantic note: "weight" refers to the weight reflected at a given mass in kilograms on earth.) It provides the option to edit or delete the pannier.

### Items

#### Index

Largely the same as the Panniers index (like all the Items views), this view iterates through all the items in panniers with the same user ID as the current user, displaying their name (with a link to that item's show page) and weight. Provides option to add a new item.

#### New

Form takes in an item's name and weight before providing options to add the item to a particular existing pannier or create a new one.

#### Edit

Allows changes to item's name, weight, and pannier (whether another existing one or a new one).

#### Show

Shows a particular item's name, weight, and pannier, with link buttons to edit or delete.

## CRUD

The above MVCs work together to create a CRUD application in Sinatra. Next up, I start tackling Rails!





