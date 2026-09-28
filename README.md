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
    // Allow admin users full access
    if (gs.hasRole('admin')) {
        return true;
    }

    // Allow only EEE branch users to see EEE records
    if (gs.hasRole('bb1')) {
        return true;
    }

    // Deny access for all others
    return false;
})();
