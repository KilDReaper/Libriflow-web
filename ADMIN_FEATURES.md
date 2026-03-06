# LibriFlow - Library Management System

## New Admin Features

This implementation adds comprehensive book management and borrowing tracking features to the LibriFlow website.

## Backend Features

### 📚 Book Management
- **Create Books**: Add new books to the library inventory with detailed information
- **Update Books**: Edit existing book information
- **Delete Books**: Remove books from the system
- **Search Books**: Search by title, author, ISBN, or category
- **Inventory Stats**: Track total books, copies, available, and borrowed

### 📋 Borrowed Books Tracking
- **Borrow Books**: Record when a user borrows a book
- **Return Books**: Mark books as returned
- **Track Due Dates**: Monitor when books are due
- **Overdue Detection**: Automatically flag overdue books
- **Borrowing History**: View complete borrowing history
- **Borrowing Stats**: Track active borrows, overdue books, and returns

## API Endpoints

### Book Routes (`/app/books`)
- `POST /` - Create a new book
- `GET /` - Get all books (optional: `?category=Fiction`)
- `GET /search?q=query` - Search for books
- `GET /inventory-stats` - Get inventory statistics
- `GET /:id` - Get a specific book by ID
- `PUT /:id` - Update a book
- `DELETE /:id` - Delete a book

### Borrowed Book Routes (`/app/borrowed-books`)
- `POST /borrow` - Borrow a book
- `PUT /return/:id` - Return a borrowed book
- `GET /` - Get all borrowed books (optional: `?status=borrowed`)
- `GET /active` - Get active borrowed books
- `GET /overdue` - Get overdue books
- `GET /stats` - Get borrowing statistics
- `GET /user/:userId` - Get borrowed books by user

## Frontend Admin Pages

### 1. Admin Dashboard (`/admin`)
- Quick overview of library stats
- Easy access to all admin features
- Visual stats cards for books, available, borrowed, and overdue counts

### 2. Book Inventory (`/admin/books`)
- View all books in a table format
- Search functionality
- Filter by category
- Inventory statistics (Total Books, Total Copies, Available, Borrowed)
- Edit and delete books directly from the list
- Status indicators (Available/Out of Stock)

### 3. Add Book (`/admin/books/create`)
Form fields:
- Title (required)
- Author (required)
- ISBN (required)
- Publisher
- Published Year (required)
- Category (required) - dropdown with predefined categories
- Total Copies (required)
- Cover Image URL
- Description

### 4. Edit Book (`/admin/books/[id]/edit`)
- Same as Add Book but pre-filled with existing data
- Additional field: Available Copies (manually adjustable)

### 5. Borrowed Books (`/admin/borrowed-books`)
- View all borrowed books with detailed information
- Filter tabs: All, Borrowed, Overdue, Returned
- Borrowing statistics
- See book details, borrower information, dates
- Visual indicators for overdue books (red background)
- Mark books as returned
- Track borrow date, due date, and return date

## Data Models

### Book Model
```typescript
{
  title: string
  author: string
  isbn: string (unique)
  publisher?: string
  publishedYear: number
  category: string
  description?: string
  totalCopies: number
  availableCopies: number
  coverImage?: string
  createdAt: Date
  updatedAt: Date
}
```

### Borrowed Book Model
```typescript
{
  bookId: ObjectId (ref: Book)
  userId: ObjectId (ref: User)
  borrowDate: Date
  dueDate: Date
  returnDate?: Date
  status: "borrowed" | "returned" | "overdue"
  createdAt: Date
  updatedAt: Date
}
```

## Architecture

The implementation follows a clean architecture pattern:

1. **Types** (`/types`) - Zod schemas for validation and TypeScript types
2. **Models** (`/models`) - MongoDB/Mongoose schemas
3. **DTOs** (`/dtos`) - Data Transfer Objects with Zod validation
4. **Repositories** (`/repositories`) - Database access layer
5. **Services** (`/services`) - Business logic layer
6. **Controllers** (`/controllers`) - HTTP request handlers
7. **Routes** (`/routes`) - API route definitions

## Features Highlights

### Inventory Management
- Track total inventory vs available copies
- Automatic copy tracking when books are borrowed/returned
- Search across multiple fields
- Category-based filtering

### Borrowing System
- Automatic due date calculation (14 days default)
- Overdue detection and flagging
- Complete borrowing history
- User-specific borrowing records
- Return tracking with dates

### User Interface
- Clean, modern design with Tailwind CSS
- Responsive layouts for mobile and desktop
- Loading states and error handling
- Confirmation dialogs for destructive actions
- Color-coded status indicators
- Real-time statistics

## Categories Available
- Fiction
- Non-Fiction
- Science
- Technology
- History
- Biography
- Self-Help
- Fantasy
- Mystery
- Romance
- Children
- Other

## Setup Instructions

### Backend
1. The backend routes are automatically configured in `src/index.ts`
2. Ensure MongoDB is running
3. The models will be automatically created on first use

### Frontend
1. The API base URL is configured in `lib/api.ts` as `http://localhost:5000/app`
2. All admin pages are under `/admin` route
3. Pages are built with Next.js and client-side rendering

## Navigation
- Main Dashboard: `/admin`
- Book Inventory: `/admin/books`
- Add Book: `/admin/books/create`
- Edit Book: `/admin/books/[id]/edit`
- Borrowed Books: `/admin/borrowed-books`
- User Management: `/admin/users`

## Next Steps / Enhancements
- Add authentication middleware to protect admin routes
- Implement role-based access control
- Add email notifications for due dates
- Implement book cover image upload
- Add fine calculation for overdue books
- Generate reports (PDF/Excel)
- Add barcode scanning support
- Implement reservation system
- Add book reviews and ratings
- Multi-language support
