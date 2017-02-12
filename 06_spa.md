# Task

Make a project of catalog-like SPA. It must have 3 types of users: buyers (explore, buy), sellers (add, sell), admins (all possible + extras) and implement basic trading with any payment provider.
We expect you to do the following:
Architecture diagram, could be UML-like;
Js classes drafts;
Detailed plan on building Access control system.

# Database structure

Here is basic part of database structure which is required for getting app works:

![Database structure](https://github.com/serhii-havrylenko/pritle/blob/master/database_structure.png)

Related on this database structure appropriate REST API should be created. CRUD operations on each table should be added.
All other business logic will be realized on client side.

# Use cases

Here is general use cases for required app:

![Use cases](https://github.com/serhii-havrylenko/pritle/blob/master/use_cases.png)

Depends on it appropriate functionality will be realized.

# Classes diagram

Here is general classes diagram, it could be changed during app realization, however all these classes and methods will help with app development:

![Classes diagram](https://github.com/serhii-havrylenko/pritle/blob/master/classes.png)

# Access control

Access control system could be built in the following way.

## Roles

Roles are defined in separate table 'Roles'. It contains only one field 'name' with datatype enum (Guest, Buyer, Seller and Admin).

Default role for all users is guest, unless user is logged in.

## ACL

ACL - separate table which contains string field only. This field describe exact element or group of elements on UI which is restricted for some roles. For example:
* items_view - allowed for all users
* item_delete - allowed only for item owner with role seller or Admin

## Access for roles

Each role has a set of ACL related to it.

### Guest

Guest user allowed to:
* view categories and items
* read comments and ranking
* compare items
* search items
* sign in/sign up
* add items to basket

### Buyer

Role for buyer includes everything from guest role plus after successful sign in:
* buy products
* edit user details
* add/modify/delete shipping addresses
* check previous orders
* track orders
* rate items
* comment on items

### Seller

Seller role has the same functionality as guest plus:
* edit user details
* check previous orders
* track orders
* rate items
* comment on items
* add/modify/delete items
* view all orders
* track orders
* gather statistic

### Admin

Admin role includes all previous roles plus extra access to the management panel where it is allowed to:
* configure payment processors
* review comments and remove them
* ban or temporary disable users
* configure ACL for different role
* manage roles
* gather statistic about payments and orders
* etc.

## How it works

Access control system has quite simple realization:
* User tries to get access to specific page
* Page contains set of elements with different ACLs for it (for example item details page - item_details_view, item_comment, item_rate, etc. )
* UI when renders each component checks user role and it's ACLs. If user's role matches to element ACL - element renders, if not - no.

With access control system like this it is easy to add or change user roles, easy to add some restricted elements to already existed pages and basically - easy to maintain and configure (Configuration available through admin panel)

# JS classes drafts

This part depends on chosen framework for UI realization. In case of realization in AngularJS (no matter which version, difference only in syntax - ECMAScript6 or typescript, base conception is still the same) all classes will be quite similar to UML classes diagrams with some extra methods for events handling, validation and so on.

## Resources

A set of similar resources will be created for interaction with REST API like:
```javascript
export class UsersFactory {
  constructor($resource, apiBaseUrl) {
    'ngInject';

    return $resource(`${apiBaseUrl}/:action/:id/`, { id: '@id', action: 'users' }, {
      update: {
        method: 'PUT'
      },
      login: {
        method: 'POST',
        params: {
          action: 'auth'
        }
      }
    });
  }
}
```
or
```javascript
export class ItemsFactory {
  constructor($resource, apiBaseUrl) {
    'ngInject';

    return $resource(`${apiBaseUrl}/items/:id`, { id: '@id' }, {
      update: {
        method: 'PUT'
      }
    });
  }
}
```

All of them will be quite similar.

## Business logic

Basically almost all business logic will be implemented inside specific services. Again, almost everything will be similar to UML classes diagram:
```javascript
export class OrdersService {
  constructor(user, OrdersFactory, order) {
    'ngInject';

    this.user = user;
    this.OrdersFactory = OrdersFactory;
    this.order = order;
  }

  getOrders() {

  }

  getUser() {

  }

  place() {

  }

  getPayment() {

  }

  getItems() {

  }

  getShippingAddress() {

  }

  changeStatus() {

  }

  getStatus() {

  }
  ...
}
```
