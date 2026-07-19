# AutomationExercise API Tests (Postman)

![Postman](https://img.shields.io/badge/API%20Tests-Postman-FF6C37)
![Newman](https://img.shields.io/badge/CLI%20Runner-Newman-00C7B7)

A Postman collection for testing the public REST API of automationexercise.com (https://automationexercise.com/api/), covering products, brands, search, login/authentication, and user account CRUD operations. Each request includes automated test assertions (status codes, response body checks, error message validation).

## Table of Contents

- What's Covered
- Endpoints Tested
- Prerequisites
- Running the Tests
- Project Structure
- Notes

## What's Covered

- Products — retrieving the product list, and confirming unsupported methods are rejected
- Brands — retrieving the brand list, and confirming unsupported methods are rejected
- Product Search — searching with and without the required parameter (happy + negative path)
- Login Verification — valid credentials, missing email, invalid credentials
- User Account Lifecycle — create account, update account, delete account, fetch user details by email

## Endpoints Tested

| Request Name | Method | Endpoint | Key Assertions |
|---|---|---|---|
| Get All Products List | GET | /api/productsList | Status 200, products list not empty |
| Post to All Products List | POST | /api/productsList | Status 200, responseCode 405 (method not supported) |
| Get All Brands List | GET | /api/brandsList | Status 200, brands list not empty |
| Put to All Brands List | PUT | /api/brandsList | Status 200, responseCode 405 (method not supported) |
| Post to Search Product | POST | /api/searchProduct | Status 200, products list not empty |
| Post to Search Product Without Parameter | POST | /api/searchProduct | Status 200, responseCode 400, "Bad Request" message |
| Post to Verify Login With Valid Details | POST | /api/verifyLogin | Status 200, message "User exists!" |
| Post to Verify Login Without Email | POST | /api/verifyLogin | Status 200, responseCode 400, missing-parameter message |
| Delete the Verified Login | DELETE | /api/verifyLogin | Status 200, responseCode 405 (method not supported) |
| Post to Verify Login With Invalid Details | POST | /api/verifyLogin | Status 200, responseCode 404, "User not found!" |
| Post to Register User Account | POST | /api/createAccount | Status 200, responseCode 201, "User created!" |
| Delete User Account | DELETE | /api/deleteAccount | Status 200, "Account deleted" |
| Put to Update User Account | PUT | /api/updateAccount | Status 200, "User updated!" |
| Get User Details by Mail | GET | /api/getUserDetailByEmail | Status 200, user object exists |

## Prerequisites

- Postman (https://www.postman.com/downloads/) to run/edit interactively, and/or
- Node.js + npm to run headlessly via Newman

## Running the Tests

Option 1 — Postman UI
1. Open Postman -> Import -> select AutomationExcercise.postman_collection.json.
2. Open the collection -> click Run to execute all requests and see pass/fail results per assertion.

Option 2 — Newman (CLI)
```
npm install -g newman
newman run AutomationExcercise.postman_collection.json
```

Add --reporters cli,html --reporter-html-export report.html to also generate an HTML report.

## Project Structure

```
├── AutomationExcercise.postman_collection.json   # The full API test collection
└── README.md
```

## Notes

- Base API URL: https://automationexercise.com/api/
- Some requests are negative-path tests (missing parameters, unsupported HTTP methods) and are expected to return non-200 response codes in the JSON body while the HTTP status itself stays 200 — this matches automationexercise.com's API behavior, not a bug in the tests.
- Test data (emails/passwords) is currently hardcoded in the collection; consider moving these to a Postman Environment before sharing or committing this publicly.
