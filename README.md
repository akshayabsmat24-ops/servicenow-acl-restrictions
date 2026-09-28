# servicenow-acl-restrictions
Script-controlled ACL to restrict record access based on specific field values.

# Script-Controlled ACL – Restrict Record Access Based on Field Value

## Project Overview

This project demonstrates how to implement a script-controlled Access Control List (ACL) in ServiceNow to restrict users from viewing records unless they meet specific conditions.

In this project, access to records in the **Institution Details** table is controlled based on the **Branch** field. Users with the required role can access the configured records, while administrators retain full access.

## Objective

The main objectives of this project are:

- To create a custom Institution Details table.
- To create users and custom roles.
- To create student records with different branch values.
- To configure READ, CREATE, WRITE, and DELETE ACLs.
- To use a script-controlled ACL to enforce record-level security.
- To restrict record access based on field values and user roles.
- To verify ACL access using different users.

## Technologies Used

- ServiceNow
- Access Control Lists (ACL)
- JavaScript
- ServiceNow User Administration
- Custom Tables and Fields

## Custom Table

### Institution Details

**Table Label:** Institution Details

**Table Name:** `u_institution_details`

The Institution Details table contains the following fields:

| Field | Type |
|---|---|
| Student Roll Number | Auto Number |
| Student Name | Reference - User |
| Faculty Name | Reference - User |
| Branch | Choice |
| Email | String |
| Phone Number | String |
| Description | Multi String |

### Branch Values

The Branch field contains the following values:

- ECE
- EEE
- CSE

## Users and Roles

A test user named **EEE User** is created for testing the ACL.

The project uses the following custom roles:

- `bb1` - Read access
- `bb2` - Create access
- `bb3` - Write access
- `bb4` - Delete access

## READ ACL

A Record - Read ACL is created for the `u_institution_details` table.

### Configuration

- **Type:** Record
- **Operation:** Read
- **Name:** `u_institution_details`
- **Active:** True
- **Advanced:** True
- **Required Role:** `bb1`
- **Data Condition:** Branch is EEE

### ACL Script

```javascript
(function () {
    if (gs.hasRole('admin')) {
        return true;
    }

    if (gs.hasRole('bb1')) {
        return true;
    }

    return false;
})();
```

## CREATE ACL

A Record - Create ACL is created for the `u_institution_details` table.

### Configuration

- **Type:** Record
- **Operation:** Create
- **Name:** `u_institution_details`
- **Active:** True
- **Required Role:** `bb2`

No data condition is added for the Create ACL.

Users with the required `bb2` role can create records.

## WRITE ACL

A Record - Write ACL is created for the `u_institution_details` table.

### Configuration

- **Type:** Record
- **Operation:** Write
- **Name:** `u_institution_details`
- **Active:** True
- **Required Role:** `bb3`

No data condition is added for the Write ACL.

Users with the required `bb3` role can modify records.

## DELETE ACL

A Record - Delete ACL is created for the `u_institution_details` table.

### Configuration

- **Type:** Record
- **Operation:** Delete
- **Name:** `u_institution_details`
- **Active:** True
- **Required Role:** `bb4`

No data condition is added for the Delete ACL.

Users with the required `bb4` role can delete records.

## Testing and Verification

The ACL configuration is tested using different users and roles.

### EEE User

The EEE user is impersonated and the Student Records list is opened.

The user with the `bb1` role is tested to verify access to the configured EEE branch records.

### User Without Required Role

A user without the required role is tested.

The ACL denies access to the records for users who do not meet the required access conditions.

### Administrator

The administrator is impersonated and the Student Records list is opened.

The administrator can view all records regardless of the branch because the ACL script provides full access.

### Create Access

A user with the required `bb2` role is tested to verify Create access.

### Write Access

A user with the required `bb3` role is tested to verify Write access.

### Delete Access

A user with the required `bb4` role is tested to verify Delete access.

## Outcome

The project demonstrates how CREATE, WRITE, and DELETE ACLs can be used to control record-level access in ServiceNow using different user roles.

## Demo Video

[Watch the Project Demo Video](https://drive.google.com/file/d/1ePuCGLWRW9taiIu0jkTpce7Ghlsa6vCp/view?usp=drivesdk)
