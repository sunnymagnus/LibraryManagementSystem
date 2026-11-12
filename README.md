# Shelfwise Library Desk

A local-first library management system for a community library in India. It runs on Node.js with no third-party packages and stores its working data in the browser's local storage.

## Run locally

1. Install Node.js if it is not already installed.
2. From this folder, run `npm start`.
3. Open [http://localhost:4173](http://localhost:4173).

Set `PORT` to use a different port. The Indian demo catalog appears on first launch. Changes are saved in the browser on the device where they were made. Use a dedicated browser profile or the app's site data controls to reset the demonstration data.

## Features

- Browse 100 seeded titles, including Indian authors; search by title, author, ISBN, or category; filter by category and availability; add more titles and shelf locations.
- Reader sign-in with a registered email and PIN. Members can borrow available books, see due dates, and return books from My account.
- Member management: create reader profiles and PINs, edit accounts, set active or inactive status, and see current borrowing counts.
- Circulation: check out and return books, enforce availability and a five-book member limit, and set the return date 14 days after checkout.
- Holds: queue a request for borrowed books and reserve the next available copy for its reader.
- Late fee of ₹5 per day, shown in the member's account and recorded when an overdue book is returned.
- Overview with collection and circulation counts, recent activity, overdue reminders, and popular titles.
- Responsive layout for desktop and mobile screens.

## Demo sign-in

- Email: `ananya.rao@example.in`
- PIN: `1234`

Other seeded members use the same demo PIN. New members receive a 4–8 digit PIN from the library desk when their account is created.

This is a browser-local demo. Member data and sign-in are not backed by a shared server, so use server-side authentication and a database before exposing the system to the public internet.

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
