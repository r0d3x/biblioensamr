# biblioensamr - API Contract

**Version: 1.0**
**Last Updated:** September 16, 2025

 This contract serves as the single source of truth for both the frontend and backend teams.

- **Base URL:** `/api`
- **Data Format:** All requests and responses will use `JSON`.
- **Authentication:** Authenticated endpoints require a `Bearer Token` in the `Authorization` header.

---

## 👤 Authentication & Users

Handles user registration, login, and profile management.

| Feature/Action | Method | URL Path | Description |
| :--- | :--- | :--- | :--- |
| **Register Account** | `POST` | `/auth/register` | Creates a new user. The backend validates the email against the pre-approved school list. **Request Body:** `{ "email": "...", "password": "...", "fullName": "..." }`. |
| **Login** | `POST` | `/auth/login` | Authenticates a user. **Request Body:** `{ "email": "...", "password": "..." }`. **Response:** `{ "accessToken": "..." }`. |
| **Get My Profile** | `GET` | `/users/me` | Fetches the profile of the currently authenticated user. (Requires Auth). |
| **Update My Profile** | `PUT` | `/users/me` | Updates profile info for the authenticated user. (Requires Auth). **Request Body:** `{ "fullName": "...", "campusLocation": "..." }`. |
| **Get Public Profile** | `GET` | `/users/{userId}` | Fetches the public profile of another user, including their name and average rating. |

---

## 📚 Books

Manages everything related to the book listings.

| Feature/Action | Method | URL Path | Description |
| :--- | :--- | :--- | :--- |
| **Get All Books** | `GET` | `/books` | Retrieves a paginated list of all available books. Supports filtering via query params (e.g., `?courseCode=CS101` or `?search=calculus`). |
| **List a New Book** | `POST` | `/books` | Adds a new book listing owned by the authenticated user. (Requires Auth). **Request Body:** `{ "title": "...", "author": "...", "courseCode": "...", "condition": "..." }`. |
| **Get Single Book** | `GET` | `/books/{bookId}` | Retrieves detailed information for a specific book. |
| **Update My Book** | `PUT` | `/books/{bookId}` | Updates the details of a book you own. (Requires Auth & Ownership). |
| **Delete My Book** | `DELETE` | `/books/{bookId}` | Removes a book listing. (Requires Auth & Ownership). |
| **Get My Listings** | `GET` | `/users/me/books` | Retrieves all books listed by the currently authenticated user. (Requires Auth). |
| **Get a User's Listings** | `GET` | `/users/{userId}/books` | Retrieves all available books listed by a specific user. |

---

## 🤝 Loans & Transactions

Handles the entire lifecycle of borrowing and lending a book.

| Feature/Action | Method | URL Path | Description |
| :--- | :--- | :--- | :--- |
| **Request to Borrow** | `POST` | `/loans` | A user requests to borrow a book. (Requires Auth). **Request Body:** `{ "bookId": "...", "message": "..." }`. |
| **Get My Loans** | `GET` | `/loans` | Gets all loan transactions involving the current user (as both borrower and lender). (Requires Auth). |
| **Accept a Request** | `PUT` | `/loans/{loanId}/accept` | The book's owner accepts a borrow request. (Requires Auth & Ownership). |
| **Reject a Request** | `PUT` | `/loans/{loanId}/reject` | The book's owner rejects a borrow request. (Requires Auth & Ownership). |
| **Confirm Handoff** | `PUT` | `/loans/{loanId}/confirm-pickup` | The borrower confirms they have received the book. This starts the loan. (Requires Auth). |
| **Confirm Return** | `PUT` | `/loans/{loanId}/confirm-return` | The lender confirms the book has been returned. This completes the loan. (Requires Auth & Ownership). |

---

## ⭐ Reviews & Ratings

Allows users to rate each other after a transaction is complete.

| Feature/Action | Method | URL Path | Description |
| :--- | :--- | :--- | :--- |
| **Submit a Review** | `POST` | `/reviews` | A user submits a rating for another user after a completed loan. (Requires Auth). **Request Body:** `{ "loanId": "...", "ratedUserId": "...", "rating": 5, "comment": "..." }`. |
| **Get User Reviews** | `GET` | `/users/{userId}/reviews` | Fetches all reviews and the average rating for a specific user. |
