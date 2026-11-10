# Shelfwise Library Desk

A local-first library management system for a community library. It runs on Node.js with no third-party packages and stores its working data in the browser's local storage.

## Run locally

1. Install Node.js if it is not already installed.
2. From this folder, run `npm start`.
3. Open [http://localhost:4173](http://localhost:4173).

Set `PORT` to use a different port. The demo catalog appears on first launch. Changes are saved in the browser on the device where they were made. Use a dedicated browser profile or the app's site data controls to reset the demonstration data.

## Features

- Catalog management: add, edit, search, filter, and remove books; track copies and shelf locations.
- Member management: add and edit readers, maintain active or inactive status, and see current borrowing counts.
- Circulation: check out and return books, enforce availability and a five-book member limit, and calculate the due date 14 days from checkout.
- Holds: place a hold when all copies are borrowed, show the waiting queue, and mark the next request ready when a copy is returned.
- Overview: collection and circulation counts, recent activity, overdue reminders, holds needing attention, and popular titles.
- Estimated overdue fine display of $0.25 per overdue day.
- Responsive layout for desktop and mobile screens.

## Hash map data model

The `LibraryStore` class in `app.js` uses JavaScript `Map` instances as hash maps:

- `books`: book ID → book record
- `booksByISBN`: ISBN → book ID, to reject duplicates and find titles quickly
- `members`: member ID → member record
- `membersByEmail`: normalized email → member ID
- `loans`: loan ID → loan record
- `loansByBook` and `loansByMember`: IDs → sets of related loan IDs
- `holdsByBook`: book ID → set of hold IDs in queue order

These indexes make ID and ISBN lookups average O(1). Relationship indexes avoid scanning every loan to count a member's active books or calculate a book's availability. A JSON snapshot of the records is serialized to local storage on each change; `Map` indexes are rebuilt at startup.

## GitHub

The project is published at [sunnymagnus/LibraryManagementSystem](https://github.com/sunnymagnus/LibraryManagementSystem). To work from a fresh local checkout, run `git clone https://github.com/sunnymagnus/LibraryManagementSystem.git` and then `npm start` from the project folder.
