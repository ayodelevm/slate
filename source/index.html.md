---
title: POST-It App

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

**`PostIt`** is a simple application that allows friends and colleagues create groups for notifications. It allows a person post notifications to everyone in his group by sending a message once.
It has the following features:

* Creating of an account
* Signing in a registerd user
* Sign-up and Sign-in through google authentication
* Reset password
* Create Groups and be added to groups by other users
* Add users to the group
* View group members
* Post messages in member groups in real-time
* Upload profile pictures
* Archive Messages and view Archived messages
* Recieve in-app, email and sms notification when a message is posted in the group you belong to, based on the message's priority level. The different priority levels are `Normal`, `Urgent`, `Critical`

# Installation and Setup

*  Clone this repository on that directory.

**`Using SSH:`**

`git clone git@github.com:ayodelevm/PostIt.git`

**`Using HTTPS:`**

`git clone https://github.com/ayodelevm/PostIt.git`

*  Navigate to the repo's folder on your computer `cd PostIt/`
* Install the app's backend dependencies using `npm install`

## System Requirements to Note
* In order to begin using, you need to have __nodeJs__ and **npm** installed on your system.
* For database you need to install __PostGres__ locally or setup with an online client eg. **ElephantSql**
* Create two (2) databases one for __development__ and the other for **testing**
* Change database config variables in the config.json file, based on your own db set-up
* In other to interact effectively with endpoints, install and use __Postman__

# Endpoints Breakdown

## Authentication Endpoints

## `Sign-up [POST - /api/v1/user/register]`
> Request Body

```json
{
  "fullname": "Toria Tobias",
  "username": "toria",
  "password": "ay324ab",
  "passwordConfirmation": "ay324ab",
  "email": "ay@gmail.com",
  "telephone": "08087584922",
}
```

This endpoint creates a new user

**`Status Code [201]`**

> Sample Response

```json
{
  token: "kfjbHJKD787ygiduewedykdubc8ebwu.Yjgkcdveucvo"
}
```

## `Login [POST - /api/v1/user/login]`
> Request Body

```json
{
  "userIdentifier": "toria" or "ay@gmail.com",
  "password": "ay324ab"
}
```

This endpoint verifies if a user is registered and then logs the user if verified or throws the appropriate error if not verified.
You can login with either username or email

**`Status Code [200]`**

> Sample Response

```json
{
  token: "kfjbHJKD787ygiduewedykdubc8ebwu.Yjgkcdveucvo"
}
```

## `Forgot Password [POST - /api/v1/user/forgotpassword]`
> Request Body

```json
{
  "email": "ay@gmail.com"
}
```

This endpoint handles verifying that a user who wants to reset password is registered and then sends a reset password link to the users email

**`Status Code [200]`**

> Sample Response

```json
{
  success: "Please check your mail for the reset link!"
}
```

## `Reset Password [PUT - /api/v1/resetpassword]`
> Request Body

```json
{
  "password": "newpassword"
  "passwordConfirmation": "newpassword"
}
```

Upon reception of an email to reset password, this method receives the new password entered by the user and replaces the user's old password with the nnew one

**`Status Code [201]`**

> Sample Response

```json
{
  success: "password reset successful, Please login to continue!"
}
```

## `Google Signup [POST - /api/v1/user/googlesignup]`
> Request Body

```json
{
  "id_token": "CDjdljdf8yg8ge092874hikudjlcedcUIYHCG.78ghGKcjdsgkj"
}
```

Upon verification of user's email by google, this endpoint receives the user's `id_token` issued by google, verifies the token and decodes it, checks if the user has previously signed-up with google in the past, if not, it saves the users detail into db including the user's unique `subject id` and then generates a new token for the user, if yes, sends an error message teeling the user he's signed-up before and asking the user to sign-in with google instead

**`Status Code [201]`**

> Sample Response

```json
{
  token: "kfjbHJKD787ygiduewedykdubc8ebwu.Yjgkcdveucvo"
}
```

## `Google Sign-in [POST - /api/v1/user/googlelogin]`
> Request Body

```json
{
  "id_token": "CDjdljdf8yg8ge092874hikudjlcedcUIYHCG.78ghGKcjdsgkj"
}
```

Upon verification of user's email by google, this endpoint recevies the user's `id_token` issued by google, verifies the token and decodes it, checks if the user is registered in the database, if yes, generates a new token for the user, if not, generates an error and tells the user to signup with google first

**`Status Code [200]`**

> Sample Response

```json
{
  token: "kfjbHJKD787ygiduewedykdubc8ebwu.Yjgkcdveucvo"
}
```

## `Verify User [POST - /api/v1/verifyuser]`
> Request Body

```json
{
  "token": "kfjbHJKD787ygiduewedykdubc8ebwu.Yjgkcdveucvo"
}
```

Upon browser refresh, or re-opening of the page, this endpoint verifies if the user's token is still valid if not, an error response is generated


**`Status Code [200]`**

> Sample Response

```json
{
  success: "user verification successful!",
  token: "kfjbHJKD787ygiduewedykdubc8ebwu.Yjgkcdveucvo"
}
```

## User Endpoints

## `Get All Registered Users [GET - /api/v1/users]`
> Sample Response

```json
{
  success: "Successful.",
  users: [{
    id: "2"
    username: "toria",
    profileImage: "https://www.conncoll.edu/media/major-images/Art.jpg",
    fullname: "Toria Tobias",
    email: "ay@gmail.com",
    telephone: "08087584922"
  }, {
    id: "3"
    username: "jide",
    profileImage: "https://www.conncoll.edu/media/major-images/Art.jpg",
    fullname: "Jide Tobias",
    email: "jide@gmail.com",
    telephone: "09074384397"
  }]
}
```

This endpoint fetches all registered users

**`Status Code [200]`**

## `Get Group Members [GET - /api/v1/group/:id/users]`
> Sample Response

```json
{
  success: "Successful.",
  foundGroupAndUsers: {
    name: "New Group",
    description: "It's a new group",
    id: "7",
    ownerId: "4",
    createdAt: "017-09-10T16:29:08.087Z",
    Users: [{
      id: "2"
      username: "toria",
      profileImage: "https://www.conncoll.edu/media/major-images/Art.jpg",
      fullname: "Toria Tobias",
      email: "ay@gmail.com",
      telephone: "08087584922"
    }, {
      id: "3"
      username: "jide",
      profileImage: "https://www.conncoll.edu/media/major-images/Art.jpg",
      fullname: "Jide Tobias",
      email: "jide@gmail.com",
      telephone: "09074384397"
    }]
  }
}
```

This endpoint fetches a group and all it's members


**`Status Code [200]`**

## `Add New Users To Group [POST - /api/v1/group/:id/user]`
> Request Body

```json
{
  "newGroupMembers": ["jide", "toria"]
}
```

This endpoint handles adding new users to a group


**`Status Code [201]`**

> Sample Response

```json
{
  success: "new users added successfully",
  addedUsers: [{
      GroupId: "2"
      UserId: "4",
      createdAt: "017-09-10T16:29:08.087Z",
      updatedAt: "017-09-10T16:29:08.087Z"
    }, {
      GroupId: "3"
      UserId: "1",
      createdAt: "017-09-10T16:29:08.087Z",
      updatedAt: "017-09-10T16:29:08.087Z"
    }]
}
```

## `Update User Details [PUT - /api/v1/user/:id/edit]`
> Request body

```json
{
  "profileImage": "https://www.conncoll.edu/media/major-images/Art.jpg"
}
```

This endpoint handles updating a user's data. Used specifically in this project for handling uploading a new profile picture

**`Status Code [200]`**

> Sample Response

```json
{
  success: "User details updated successfully.",
}
```

## Group Endpoints

## `Get One User And All His Groups [GET - /api/v1/groups]`
> Sample Response

```json
{
  success: 'Successful.',
  foundUserAndGroups: {
    id: "2"
    username: "toria",
    profileImage: "https://www.conncoll.edu/media/major-images/Art.jpg",
    fullname: "Toria Tobias",
    email: "ay@gmail.com",
    telephone: "08087584922",
    Groups: [{
      name: "New Group",
      description: "It's a new group",
      id: "7",
      ownerId: "4",
      createdAt: "017-09-10T16:29:08.087Z",
    }, {
      name: "A Second Group",
      description: "A second for demo",
      id: "8",
      ownerId: "4",
      createdAt: "017-09-10T16:29:08.087Z",
    }]
  }
}
```

This endpoint fetches one user and all the group he belongs to

**`Status Code [200]`**

## `Create A New Group [POST - /api/v1/group]`
> Request Body

```json
{
  "name": "Learn Python",
  "description": "We teach python!",
  "initialGroupMembers": ["toria", "jide"]
}
```

This endpoint handles creating a new group and adding new users to the group at the point of creating the group

**`Status Code [201]`**

> Sample Response

```json
{
  success: "New group created successfully.",
  newGroup: {
    name: "Learn Python",
    description: "We teach python!",
    id: "9",
    ownerId: "4",
    createdAt: "017-09-10T16:29:08.087Z",
  }
}
```

## Other Group Endpoints

<aside class="notice">
<b>Note:</b> Provision were made for the following <b>group endpoints</b> but were not implemented in the client-side for this sprint/ version
</aside>

## `Get One Group To Edit [GET - /api/v1/group/:id/edit]`
> Sample Response

```json
{
  success: "Successful.",
  foundGroup: {
    name: "Learn Python",
    description: "We teach python!",
    id: "9",
    UserId: "4",
    createdAt: "017-09-10T16:29:08.087Z",
  }
}
```

This endpoint handles fetching a group data for editing

**`Status Code [200]`**

## `Update One Group [PUT - /api/v1/group/:id/edit]`
> Request Body

```json
{
  "name": "Learn Pythonic- Python"
}
```

This endpoint handles updating a group's details

**`Status Code [200]`**

> Sample Response

```json
{
  success: "Group details updated successfully."
}
```

## `Delete A Group [DELETE - /api/v1/group/:id/delete]`
> Sample Response

```json
{
  success: "Group deleted successfully."
}
```

This endpoint handles deleting a group along with it's messages and users

**`Status Code [200]`**

## Message Endpoints

## `Get One Group And It's Messages [GET - /api/v1/group/:id/messages]`
> Sample Response

```json
{
  success: "Successful.",
  foundGroupAndMessages: {
    name: "New Group",
    description: "It's a new group",
    id: "7",
    ownerId: "4",
    createdAt: "017-09-10T16:29:08.087Z",
    Users: [{
      id: "2"
      GroupId: "2"
      ownerId: "4",
      createdAt: "017-09-10T16:29:08.087Z",
      message: "Hello World!",
      priority: "Normal"
    }, {
      id: "3"
      GroupId: "3"
      ownerId: "5",
      createdAt: "017-09-10T16:29:08.087Z",
      message: "Welcome to this group!",
      priority: "Urgent"
    }]
  }
}
```

This endpoint handles getting one group and all it's messages. It accepts a query parameter `?archived=false` to retrieve only messages that have not been archived and `?archived=true` to retrieve only messages that have been archived

**`Status Code [200]`**


## `Create A New Message [POST - /api/v1/group/:id/message]`
> Request Body

```json
{
  "message": "Welcome to this group"
  "priority": "Critical"
}
```

This endpoint handles posting (creating) a new message to a group, if you are a member of the group

**`Status Code [201]`**

> Sample Response

```json
{
  success: "New message added successfully.",
  createdMessage: {
    id: "4"
    GroupId: "3"
    ownerId: "5",
    createdAt: "017-09-10T16:29:08.087Z",
    message: "Welcome to this group!",
    priority: "Urgent"
  }
}
```

## `Archive All Messages [PUT - /api/v1/group/:id/archivemessages]`
> Request Body

```json
{
  "messageIds": [1, 3, 2, 5]
}
```

This method handles archiving all the messages in a group 

**`Status Code [200]`**

> Sample Response

```json
{
  success: "Messages have been archived successfully",
}
```

## Note

<aside class="notice">
In using this endpoints, the following should be noted: <br>
<ul>
  <li> An authorization header needs to be attached when using users, groups and messages endpoints and including when verifying user. The format is:
  <b><i>{ Authorization: "Bearer `${token}` }</i></b></li>
  <li> You can only add users to a group you created</li>
  <li> You can only update your own user details</li>
  <li> You can only archive messages in a group you created</li>
  <li> You can only edit a group you created</li>
  <li> You can only delete a group you created</li>
</ul>
</aside>
