---
title: Arvofinance

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href="#">Sign Up for a Developer Key</a>
  - <a href="https://github.com/tripit/slate">Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

**`Arvofinance`** a loan disbursment platform. Here"s the list of the breakdown of the Api"s for the platform:

# Installation and Setup

*  Clone this repository on that directory.

**`Using SSH:`**

`git clone git@github.com:ayodelevm/slate.git`

**`Using HTTPS:`**

`git clone https://github.com/ayodelevm/slate.git`

*  Navigate to the repo"s folder on your computer `cd arvofinance-api/`
* Install the app"s backend dependencies using `npm install`

## System Requirements to Note
* In order to begin using, you need to have __nodeJs__ and **npm** installed on your system.
* For database you need to install __PostGres__ locally or setup with an online client eg. **ElephantSql**
* Create two (2) databases one for __development__ and the other for **testing**
* Change database config variables in the config.json file, based on your own db set-up
* In other to interact effectively with endpoints, install and use __Postman__

# Customer Endpoints

## `Sign-up [POST - /api/v1/customer/signup]`
> Request Body

```json
{
  "firstName": "Toria Tobias",
  "lastName": "toria",
  "password": "ay324ab",
  "confirmPassword": "ay324ab",
  "email": "name@mail.com",
  "telephone": "08087583456",
}
```

This endpoint creates a new individual customer

**`onSuccess - Status Code [201]`**

> Sample Response

```json
{
  "data": {
    "id": "A7BNKDS87KIHSL",
    "email": "name@mail.com"
  },
  "message": "Check your phone or email for otp",
  "success": true
}
```

## `Verify-OTP [POST - /api/v1/customer/verifyotp?email="name@mail.com"&id="A7BNKDS87KIHSL"]`
> Request Body

```json
{
  "token": "456789",
  "success": true
}
```

This endpoint sends an OTP token to the customers phone number and email after signup

**`onSuccess - Status Code [200]`**

> Sample Response

```json
{
  "user": {
    "id": "A7BNKDS87KIHSL",
    "email": "name@mail.com"
    ...
  },
  "token": "Xtydsj8fDSHJFfs.Hhhsjhwyi99eb.JGJDs0djwjfc2",
  "success": true
}
```

## `Resend-OTP [GET - /api/v1/customer/resendotp]`
> Response Body

```json
{
  "token": "456789",
  "success": true
}
```

This endpoint sends an OTP token to the customers phone number and email after signup

**`onSuccess - Status Code [200]`**


## `Login [POST - /api/v1/customer/login]`
> Request Body

```json
{
  "email": "toria" or "ay@gmail.com",
  "phoneDigits" "3456",
  "password": "ay324ab"
}
```

This endpoint verifies if a customer is registered and then logs the customer if verified or throws the appropriate error if not verified.
N.B: If the

**`onSuccess - Status Code [200]`**

> Sample Response (Application Not Complete)

```json
{
  "user": {
    "id": "A7BNKDS87KIHSL",
    "email": "name@mail.com"
    ...
  },
  "token": "kfjbHJKD787ygiduewedykdubc8ebwu.Yjgkcdveucvo",
  "success": true
}
```

> Sample Response (Application Complete)

```json
{
  "data": {
    "id": "A7BNKDS87KIHSL",
    "email": "name@mail.com"
  },
  "message": "Check your phone or email for otp",
  "success": true
}
```

## `Save Personal Details [PUT - /api/v1/customer/savepersonaldetails]`
> Request Body

```json
{
   "title": "tope",
   "gender": "female",
   "surname": "Ogunlade",
   "firstName": "Iyanu",
   "otherNames": "Tunde",
   "dateOfBirth": "12-may-2000",
   "maritalStatus": "Single",
   "telephone": "09043894345",
   "personalEmail": "titi@mail.com",
   "homeAddress": "2, tokunbo street, Lagos",
   "state": "Oyo",
   "lga": "Ibadan"
}
```

This endpoint handles updating a users personal details

**`onSuccess - Status Code [200]`**

> Sample Response

```json
{
  "message": "record updated succesfully!",
  "success": true
}
```

## `Create/ Update Employment Record [POST - /api/v1/customer/createemploymentrecord]`
> Request Body

```json
{
  "employmentStatus": "employed",
  "occupation": "teaching",
  "designation": "",
  "employerName": "Ibim Securities",
  "timeInCurrentEmployment": "6", //In months
  "officeEmail": "tunde@ibimsecuritiescom",
  "officeAddress": "3, Kosofe, Lagos",
  "officeState": "Lagos",
  "officeLga": "Kosofe",
  "jobsInLastFiveMonths": 2,
  "netMonthlyIncome": "80,000",
  "extraIncomeSource": "Yes"
}
```

This endpoint handles creating a new employment record for customer if not exists or updating an existing one

**`onSuccess - Status Code [201]`**

> Sample Response

```json
{
  "employmentRecord": {
    "employmentStatus": "employed",
    "occupation": "teaching",
    ...
  },
  "success": true
}
```

## `Request Loan/ Update Loan Request [POST - /api/v1/customer/requestloan]`
> Request Body

```json
{
  "loanType": "payday loan",
  "loanAmount": 20000,
  "loanPurpose": "Personal",
  "loanTenor": "4", //In months
  "modeOfRepayment": "Direct debit",
  "existingLoans": true,
  "bvn": "09855479294",
  "accountName": "Tope Ogunlade",
  "accountNumber": "64873492",
  "bank": "Union Bank",
  "typeOfAccount": "savings",
  "creditAmount": 19400,
  "capitalRepayment": 30000,
  "interestRepayment": 6000,
  "monthlyRepayment": 9000,
  "totalRepayment": 36000,
  "interestOnLoan": 4 //In percentage
}
```

Endpoint handles creating and updating record for a loan request.
N.B. A customer can only update a loan request that is still pending and a customer can have only one active loan at a time. Once a loan has been approved, the customer can no longer update it neither can he request another loan until he has paid off the current loan

**`onSuccess - Status Code [201]`**

> Sample Response

```json
{
  "loanRequest": {
    "loanType": "payday loan",
    "loanAmount": 20000,
    "loanPurpose": "Personal",
    ...
  },
  "success": true
}
```

## `Add Uploads [POST - /api/v1/customer/adduploads]`
> Request Body

```json
{
  "id_token": "CDjdljdf8yg8ge092874hikudjlcedcUIYHCG.78ghGKcjdsgkj"
}
```

Upon verification of user"s email by google, this endpoint recevies the user"s `id_token` issued by google, verifies the token and decodes it, checks if the user is registered in the database, if yes, generates a new token for the user, if not, generates an error and tells the user to signup with google first

**`onSuccess - Status Code [200]`**

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

Upon browser refresh, or re-opening of the page, this endpoint verifies if the user"s token is still valid if not, an error response is generated


**`onSUccess - Status Code [200]`**

> Sample Response

```json
{
  success: "user verification successful!",
  token: "kfjbHJKD787ygiduewedykdubc8ebwu.Yjgkcdveucvo"
}
```

# User Endpoints

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

**`onSuccess - Status Code [200]`**

## `Get Group Members [GET - /api/v1/group/:id/users]`
> Sample Response

```json
{
  success: "Successful.",
  foundUsers: {
    name: "New Group",
    description: "It"s a new group",
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

This endpoint fetches a group and all it"s members


**`onSuccess - Status Code [200]`**

## `Add New Users To Group [POST - /api/v1/group/:id/user]`
> Request Body

```json
{
  "members": ["jide", "toria"]
}
```

This endpoint handles adding new users to a group


**`onSuccess - Status Code [201]`**

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

## `Upload Profile Picture [PUT - /api/v1/user/:id/edit]`
> Request body

```json
{
  "profileImage": "https://www.conncoll.edu/media/major-images/Art.jpg"
}
```

This endpoint handles uploading a new profile picture. It can be extended for updating users details

**`onSuccess - Status Code [200]`**

> Sample Response

```json
{
  success: "User details updated successfully.",
}
```

# Group Endpoints

## `Get One User And All His Groups [GET - /api/v1/groups]`
> Sample Response

```json
{
  success: "Successful.",
  foundGroups: {
    id: "2"
    username: "toria",
    profileImage: "https://www.conncoll.edu/media/major-images/Art.jpg",
    fullname: "Toria Tobias",
    email: "ay@gmail.com",
    telephone: "08087584922",
    Groups: [{
      name: "New Group",
      description: "It"s a new group",
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

**`onSuccess - Status Code [200]`**

## `Create A New Group [POST - /api/v1/group]`
> Request Body

```json
{
  "name": "Learn Python",
  "description": "We teach python!",
  "members": ["toria", "jide"]
}
```

This endpoint handles creating a new group and adding new users to the group at the point of creating the group

**`onSuccess - Status Code [201]`**

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

**`onSuccess - Status Code [200]`**

## `Update One Group [PUT - /api/v1/group/:id/edit]`
> Request Body

```json
{
  "name": "Learn Pythonic- Python"
}
```

This endpoint handles updating a group"s details

**`onSuccess - Status Code [200]`**

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

This endpoint handles deleting a group along with it"s messages and users

**`onSuccess - Status Code [200]`**

# Message Endpoints

## `Get One Group And It"s Messages [GET - /api/v1/group/:id/messages]`
> Sample Response

```json
{
  success: "Successful.",
  foundMessages: {
    name: "New Group",
    description: "It"s a new group",
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

This endpoint handles getting one group and all it"s messages. It accepts a query parameter `?archived=false` to retrieve only messages that have not been archived and `?archived=true` to retrieve only messages that have been archived

**`onSuccess - Status Code [200]`**


## `Create A New Message [POST - /api/v1/group/:id/message]`
> Request Body

```json
{
  "message": "Welcome to this group"
  "priority": "Critical"
}
```

This endpoint handles posting (creating) a new message to a group, if you are a member of the group

**`onSuccess - Status Code [201]`**

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

**`onSuccess - Status Code [200]`**

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
