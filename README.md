# Lab 3: REST API, Query Parameters, Validation and Debugging

## Student Details

- **Name:** Sujith Kumar
- **Registration Number:** 252U1R1178
- **Section:** CSE 2D
- **Course:** Backend Development Fundamentals · Year II Term II

---

## Objective

To develop RESTful APIs using Express.js and SQLite by implementing search, sorting, pagination, price filtering, request validation, proper HTTP status codes, and consistent error handling. The objective also includes testing API endpoints using Postman and debugging common backend development errors.

---

## Technologies Used

- **Node.js** (v18+)
- **Express.js** (REST API framework)
- **SQLite** (`sqlite` + `sqlite3`)
- **Postman** (API testing & automated test verification)
- **Git and GitHub** (Version control)

---

## Project Structure

```text
lab-03/
├── app.js                              # Express application, routes, and SQLite database setup
├── server.js                           # Alternative CommonJS server entry point
├── package.json                        # Dependencies, scripts, and ES Module config
├── vercel.json                         # Cloud deployment specification for Vercel
├── .gitignore                          # Excludes node_modules/, books.db, .env
├── README.md                           # Complete lab documentation with embedded screenshots
├── TEST_RESULTS.md                     # Detailed API test execution report
├── test-api.js                         # 29 automated test assertions
├── api/
│   └── index.js                        # Vercel serverless entry point
├── routes/
│   └── books.js                        # Books router (CommonJS)
├── debugging-exercise/
│   ├── broken-server.js                # Broken exercise code with 4 classic bugs
│   ├── fixed-server.js                 # Resolved debugging implementation
│   └── DEBUGGING.md                    # In-depth debugging analysis & solutions
└── screenshots/                        # Postman test execution evidence
    ├── get-all-books-1.png
    ├── get-all-books-2.png
    ├── search-books.png
    ├── sort-books.png
    ├── pagination.png
    ├── price-filter.png
    ├── post-book.png
    ├── put-book.png
    └── delete-book.png
```

> **Note:** The `books.db` database and `node_modules/` directory are kept locally and excluded from GitHub using `.gitignore`. The database is automatically initialized and seeded by `app.js` upon server startup.

---

## Installation and Setup

### Prerequisites
- Node.js (v18+ recommended)
- Postman for endpoint testing

### 1. Install Dependencies
```bash
npm install
```

### 2. Start the Server
```bash
npm start
```
The server will initialize the SQLite database, seed 10 sample books, and start listening at:
`http://localhost:3000`

---

## Database Details

- **Database File:** `books.db` (SQLite)
- **Table Name:** `book`

### Table Schema

| Column | Data Type | Constraints | Description |
|---|---|---|---|
| `id` | INTEGER | PRIMARY KEY AUTOINCREMENT | Unique book ID |
| `title` | TEXT | NOT NULL | Title of the book |
| `author` | TEXT | NOT NULL | Author's full name |
| `price` | INTEGER | NOT NULL | Price of the book in INR |
| `pages` | INTEGER | NOT NULL | Number of pages |

### Seeded Sample Data (10 Books)
1. **The Alchemist** – Paulo Coelho (₹250, 208 pages)
2. **Wings of Fire** – A. P. J. Abdul Kalam (₹300, 180 pages)
3. **Atomic Habits** – James Clear (₹450, 320 pages)
4. **The Hobbit** – J. R. R. Tolkien (₹350, 310 pages)
5. **1984** – George Orwell (₹200, 328 pages)
6. **Clean Code** – Robert C. Martin (₹600, 464 pages)
7. **The Great Gatsby** – F. Scott Fitzgerald (₹220, 180 pages)
8. **Rich Dad Poor Dad** – Robert Kiyosaki (₹280, 336 pages)
9. **Think and Grow Rich** – Napoleon Hill (₹240, 250 pages)
10. **Ikigai** – Hector Garcia (₹260, 194 pages)

---

## REST API Endpoints Specification

**Base URL:** `http://localhost:3000`

| Method | Endpoint | Description | Status Code |
|---|---|---|---|
| `GET` | `/` | Health and API index check | `200 OK` |
| `GET` | `/books` | Retrieve books with search, sort, filter & pagination | `200 OK` / `400 Bad Request` |
| `GET` | `/books/:bookId` | Retrieve a single book by ID | `200 OK` / `404 Not Found` |
| `POST` | `/books` | Add a new book (validated payload) | `201 Created` / `400 Bad Request` |
| `PUT` | `/books/:bookId` | Update an existing book | `200 OK` / `404 Not Found` |
| `DELETE` | `/books/:bookId` | Delete a book by ID | `200 OK` / `404 Not Found` |

---

### Detailed Route Specifications

#### 1. Get All Books
- **Request:** `GET /books`
- **Response:** `200 OK` with JSON `{ "data": [...], "count": 10 }`

#### 2. Search Books (`search_q`)
- **Request:** `GET /books?search_q=atomic`
- **SQL:** `WHERE title LIKE ?` with `%atomic%`
- **Response:** Returns all books containing "atomic" in the title.

#### 3. Sort Books (`order_by`, `order`)
- **Request:** `GET /books?order_by=price&order=DESC`
- **Allowed `order_by`:** `id`, `title`, `author`, `price`, `pages`
- **Allowed `order`:** `ASC`, `DESC`
- **Response:** Books sorted by price, highest first.

#### 4. Pagination (`limit`, `offset`)
- **Request:** `GET /books?limit=3&offset=0`
- **Default:** `limit=10`, `offset=0`
- **Response:** First page with 3 books.

#### 5. Price Range Filter (`min_price`, `max_price`)
- **Request:** `GET /books?min_price=200&max_price=400`
- **SQL:** `WHERE price BETWEEN ? AND ?`
- **Response:** Books priced between ₹200 and ₹400.

---

## Screenshots

### 1. GET /books – All Books (Page 1)
![GET All Books - Page 1](screenshots/get-all-books-1.png)

### 2. GET /books – All Books (Page 2)
![GET All Books - Page 2](screenshots/get-all-books-2.png)

### 3. GET /books?search_q=... – Search Books
![Search Books](screenshots/search-books.png)

### 4. GET /books?order_by=price&order=DESC – Sort Books
![Sort Books](screenshots/sort-books.png)

### 5. GET /books?limit=3&offset=0 – Pagination
![Pagination](screenshots/pagination.png)

### 6. GET /books?min_price=...&max_price=... – Price Filter
![Price Filter](screenshots/price-filter.png)

### 7. POST /books – Create a Book
![POST Book](screenshots/post-book.png)

### 8. PUT /books/:bookId – Update a Book
![PUT Book](screenshots/put-book.png)

### 9. DELETE /books/:bookId – Delete a Book
![DELETE Book](screenshots/delete-book.png)

---

## Debugging Exercise

Four classic backend bugs were identified and resolved. See [`debugging-exercise/DEBUGGING.md`](debugging-exercise/DEBUGGING.md) for full details.

| # | Bug Type | Error Symptom | Root Cause | Fix |
|---|---|---|---|---|
| 1 | **Port Conflict** | `EADDRINUSE: address already in use :::3000` | Hardcoded busy port without fallback | Use `process.env.PORT \|\| 3001`; handle `EADDRINUSE` dynamically |
| 2 | **Wrong Import Path** | `Cannot find module './db/connection'` | Wrong relative path in `require()` | Fixed to `../database` |
| 3 | **Table-Name Typo** | `SQLITE_ERROR: no such table: book` | Query used `book` instead of `books` | Corrected table name to `books` |
| 4 | **Missing `await`** | Response returns `{}` instead of data | `db.all()` is async; without `await`, Promise is serialized | Added `await` before async DB calls |

---

## Viva Voce Questions & Answers

### 1. Query parameter vs Path parameter?
- **Path Parameters (`req.params`):** Part of the URL route itself (e.g. `/books/:bookId` → `/books/1`). Used to identify a **specific, unique resource**. Mandatory for the endpoint to match.
- **Query Parameters (`req.query`):** Key-value pairs following `?` in the URL (e.g. `?search_q=atomic&limit=5`). Used for **filtering, searching, sorting, and pagination** over collections. Optional, with default fallback values.

### 2. What does stateless mean in REST?
- **Statelessness** means the server **retains no client session state** between successive requests.
- Each client request must contain all the context and information (headers, parameters, payload, credentials) required for the server to process it.
- **Benefits:** Simplifies server architecture, allows straightforward horizontal scaling, and enables seamless load balancing.

### 3. Which status code for bad input vs a missing record?
- **`400 Bad Request`:** Used for **bad input** (client-side errors like missing required fields, non-numeric price, negative page counts, or malformed JSON syntax).
- **`404 Not Found`:** Used for a **missing record** (syntactically valid request, but the requested resource ID does not exist in the database).

---

## Automated Test Verification

Run the built-in automated test suite:
```bash
npm test
```
**Output (29/29 Passed):**
```text
==================================================
   LAB 3 REST API AUTOMATED TEST SUITE
==================================================

Database initialized with 10 sample books.
Books API running on port 3040
[Test Suite 1: Server Health]
  PASS: GET / returns 200 OK
  PASS: GET / returns welcome message

[Test Suite 2: GET /books Query Parameters & Filtering]
  PASS: GET /books returns 200 OK
  PASS: GET /books returns data array and count
  PASS: GET /books?search_q=atomic returns 200 OK
  PASS: Search filters books with title matching 'atomic'
  PASS: GET /books?order_by=price&order=DESC returns 200 OK
  PASS: Books are sorted descending by price
  PASS: GET /books?limit=3&offset=0 returns 200 OK
  PASS: Limit respects count of 3
  PASS: GET /books with min_price=100&max_price=500 returns 200 OK
  PASS: All books match BETWEEN 100 and 500 price range
  PASS: GET /books?min_price=invalid returns 400 Bad Request

[Test Suite 3: GET /books/:bookId]
  PASS: GET /books/1 returns 200 OK
  PASS: Returns correct book with id 1
  PASS: GET /books/99999 returns 404 Not Found
  PASS: Returns message: 'Book not found'
  PASS: GET /books/abc returns 400 Bad Request

[Test Suite 4: POST /books (Create & Validation)]
  PASS: POST /books returns 201 Created
  PASS: Returns success message and created book
  PASS: POST /books missing title returns 400 Bad Request
  PASS: POST /books missing author returns 400 Bad Request
  PASS: POST /books non-numeric price returns 400 Bad Request

[Test Suite 5: PUT /books/:bookId (Update)]
  PASS: PUT /books/1 returns 200 OK after update
  PASS: Updated book reflects new title and price
  PASS: PUT /books/99999 returns 404 Not Found

[Test Suite 6: DELETE /books/:bookId (Delete)]
  PASS: DELETE /books/:id returns 200 OK after delete
  PASS: Returns confirmation message
  PASS: DELETE /books/:id on non-existent id returns 404 Not Found

==================================================
TOTAL TESTS: 29
PASSED: 29
FAILED: 0
==================================================
```

---

## Conclusion

This project completely fulfills all objectives of Lab 3:
- RESTful noun endpoints with HTTP methods (`GET`, `POST`, `PUT`, `DELETE`).
- Partial search with `LIKE`, sorting, pagination, and price-range filtering using parameterised queries.
- Strict request validation returning `400 Bad Request` with consistent JSON messages.
- Correct status codes (`200`, `201`, `400`, `404`, `500`).
- Complete resolution of four classic backend errors (`EADDRINUSE`, bad import paths, table-name typos, and missing `await`).
- Testing evidence matched with Postman screenshots.

