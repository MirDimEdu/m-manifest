# System Manifest

Description of all micro-services

## Table of content

- [Auth micro-service](#auth-micro-service)
    - [General info](#Auth-General-info)
    - [External API](#Auth-External-API)
    - [Internal API](#Auth-Internal-API)
- [Accounts micro-service](#accounts-micro-service)
    - [General info](#Accounts-General-info)
    - [External API](#Accounts-External-API)
    - [Internal API](#Accounts-Internal-API)

## Auth micro-service

### Auth General info

- Host (local / aws): 0.0.0.0 / 
- Post: 8001
- Domain (local / aws): localhost /
- AUTH_TOKEN_NAME: X-MIRDIMEDU-Token

### Auth External API

1.  `/is_auth` [POST]:
    -   Check auth
    -   IN:
        -   `name` - token name in cookies
        -   `data` - token data
    -   OUT:
        -   `200 OK`
        -   `401 Unauthorized` with description

2.  `/login` [POST]:
    -   Login user
    -   IN:
        -   `login`
        -   `password`
    -   OUT:
        -   `200 OK` with `set-cookie`
        -   `404 User not found`
        -   `422 Incorrect password`

3.  `/logout` [GET]:
    -   Logout user
    -   OUT:
        -   `200 OK` with `delete-cookie`
        -   `401 Unauthorized`

4.  `/close_other_sessions` [POST]:
    -   Close other session
    -   IN:
        -   `password`
    -   OUT:
        -   `200 OK`
        -   `401 Unauthorized`
        -   `422 Incorrect password`

### Auth Internal API

1.  `/delete_other_sessions` [POST]:
    -   Close other sessions
    -   IN:
        -   `session`
        -   `account_id`
    -   OUT:
        -   `200 OK`

2.  `/delete_all_sessions` [POST]:
    -   Close all sessions
    -   IN:
        -   `session`
        -   `account_id`
    -   OUT:
        -   `200 OK`

## Accounts micro-service

### Accounts General info

- Host (local / aws): 0.0.0.0 / 
- Post: 8002

### Accounts External API

1.  `/register` [POST]:
    -   Register account
    -   IN:
        -   `login`
        -   `password`
    -   OUT:
        -   `201 OK`
        -   `401 Unauthorized` with description
        -   `409 User with this login already exists`

2.  `/me` [GET] - LOGIN REQUIRED:
    -   Get account info
    -   OUT:
        -   `200 OK` with account info (login, role, client, session_issued)

3.  `/change_password` [POST] - LOGIN REQUIRED:
    -   Change password and close other sessions
    -   IN:
        -   `old_password`
        -   `new_password`
    -   OUT:
        -   `200 OK`
        -   `401 Unauthorized`
        -   `422 Old and new passwords are equal`
        -   `422 Incorrect password`


### Accounts Internal API

1.  `/verify_account` [POST]:
    -   Verify account via login+password / account_id
    -   IN:
        -   `login` [OPTIONAL]
        -   `account_id` [OPTIONAL]
        -   `password`
    -   OUT:
        -   `201 OK`
        -   `401 Not verified`
