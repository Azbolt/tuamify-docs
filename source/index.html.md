---
title: Lahem Documentation Reference

toc_title: API Docs

language_tabs:
  - graphql

toc_footers:
  - <a href='https://github.com/Azbolt/tuamify-docs'>Documentation hosted at GitHub</a>

search: true

code_clipboard: true

meta: 
  - name: description 
  - content: Documentation for Lahem

---

# User

<aside class="notice">
  Please view <a href="https://www.notion.so/tuamify/User-cbb56104560445e699584e2492b0f520" target="_blank">User model</a> in data dictionary.
</aside>

## Permissions

Role|insert|select|update|delete
----|------|------|------|------
admin|Yes|Yes|Yes|Yes
owner|No|Only users belonging to same restaurant|No|No
user|No|Only own user record|Can only update `password` and `name` columns|No
anon|Yes|No|No|No

## Create a user

```graphql
mutation insertUserOne($email: String = "", $name: String = "", $password: String = "", $phone_number: String = "", $restaurant_id: uuid = "") {
  UserCreate(password: $password, phone_number: $phone_number, restaurant_id: $restaurant_id, email: $email, name: $name) {
    id
    message
  }
}
```

Creates a new user. All parameters are required.

<aside class="notice">
  Insert permissions apply.
</aside>

## Get users by restaurant

```graphql
query getUsersByRestaurant($_eq: uuid = "") {
  tuamify_User(where: {restaurant_id: {_eq: $_eq}}) {
    restaurant_id
    phone_number
    name
    id
    is_admin
    email
    created_at
  }
}
```

Returns a list of users belonging to the restaurant with given id.

<aside class="notice">
  Select permissions apply.
</aside>

## Return user by Id

```graphql
query getUserById($id: uuid = "") {
  tuamify_User_by_pk(id: $id) {
    created_at
    email
    restaurant_id
    phone_number
    name
    is_admin
    id
  }
}
```

Returns a user object with given id.

<aside class="notice">
  Select permissions apply.
</aside>

# Address

<aside class="notice">
  Please view <a href="https://www.notion.so/tuamify/Address-6a03a692dd67427787a8d3dded69bd1a" target="_blank">Address model</a> in data dictionary.
</aside>

## Permissions

Role|insert|select|update|delete
----|------|------|------|------
admin|Yes|Yes|Yes|Yes
owner|No|Only addresses of the users belonging to same restaurant|Can only update `name`, `longitude`, `latitude` and `street_address` columns of addresses of the users belonging to same restaurant|No
user|No|Only addresses belonging to own user record|Can only update `name`, `default`, `longitude`, `latitude` and `street_address` columns of addresses belonging to own user record|Only addresses belonging to own user record
anon|No|No|No|No

## Create an address

```graphql
mutation insertAddressOne($set: tuamify_Address_insert_input!) {
  insert_tuamify_Address_one(object: $set) {
    default
    id
    latitude
    longitude
    name
    street_address
    user_id
  }
}
```

Creates a new address. Please view data dictionary to see required fields.

<aside class="notice">Insert permissions apply.</aside>

## Update an address

## Delete an address

```graphql
mutation deleteAddressById($id: uuid = "") {
  delete_tuamify_Address_by_pk(id: $id) {
    default
    id
    latitude
    longitude
    name
    user_id
    street_address
  }
}
```

Deletes an address with given id.

<aside class="notice">Delete permissions apply</aside>

## Get address by user id

```graphql
query getAddressByUserId($user_id: uuid = "") {
  tuamify_Address(where: {user_id: {_eq: $user_id}}) {
    default
    latitude
    id
    longitude
    name
    street_address
    user_id
  }
}
```

Returns a list of addresses belonging to user with given id.

<aside class="notice">Select permissions apply</aside>

# Menu Category

<aside class="notice">
  Please view <a href="https://www.notion.so/tuamify/MenuCategory-e1aac987a24044cfa08ca9892ba3b8fa" target="_blank">Menu Category model</a> in data dictionary.
</aside>

## Permissions

Role|insert|select|update|delete
----|------|------|------|------
admin|Yes|Yes|Yes|Yes
owner|Can only insert `name` column|Only menu categories belonging to same restaurant|Can only update `name` column of menu categories belonging to same restaurant|Only menu categories belonging to same restaurant
user|No|Only menu categories belonging to same restaurant|No|No
anon|No|Yes|No|No

## Create a menu category

## Update a menu category

## Delete a menu category

## Subscribe to whole menu by restaurant id

```graphql
subscription subscribeMenuByRestaurantId {
  tuamify_MenuCategory {
    id
    name
    MenuItems {
      price
      photo
      name
      menu_category_id
      id
      description
      MenuItemVariationGroups {
        required
        menu_item_id
        name
        allow_multiple_selection
        id
        MenuItemVariations {
          price
          name
          menu_item_variation_group_id
          id
          description
        }
      }
    }
    restaurant_id
  }
}
```

Returns a list of menu categories belonging to restaurant with given id. Each menu category has nested menu items. Each
menu item has nested menu item variation groups. Each menu item variation group has nested menu item variations.

<aside class="notice">Select permissions apply.</aside>

