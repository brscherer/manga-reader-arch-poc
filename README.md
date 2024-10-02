# Manga Reader - Architecture POC

Defining some Architectural concepts in a hypothetical system to explore architecture skills

```
manga-reader-backend/
├── src/
│   ├── config/                 # Application-wide configuration (e.g., AWS, database connections)
│   ├── middlewares/            # Global middlewares (e.g., authentication, logging)
│   ├── common/                 # Reusable utilities, helpers, constants
│   ├── users/                  # Feature: Users
│   │   ├── userController.js    # Handle user-related HTTP requests
│   │   ├── userService.js       # Business logic for users (e.g., login, register)
│   │   ├── userModel.js         # Database models for users (e.g., DynamoDB schema)
│   │   ├── userRoutes.js        # API routes for users (e.g., /api/users)
│   ├── mangas/                 # Feature: Mangas and chapters
│   │   ├── mangaController.js   # Handle manga-related HTTP requests
│   │   ├── mangaService.js      # Business logic for mangas (e.g., fetch chapters)
│   │   ├── mangaModel.js        # Database models for manga (e.g., PostgreSQL schema)
│   │   ├── mangaRoutes.js       # API routes for mangas (e.g., /api/mangas)
│   ├── comments/               # Feature: Comments
│   │   ├── commentController.js # Handle comment-related HTTP requests
│   │   ├── commentService.js    # Business logic for comments
│   │   ├── commentModel.js      # Database models for comments (e.g., DynamoDB schema)
│   │   ├── commentRoutes.js     # API routes for comments (e.g., /api/comments)
│   ├── app.js                  # Main entry point for the server (Express setup)
│   ├── server.js               # Server startup and listening
├── tests/                      # Unit and integration tests
│   ├── users/                  # Test files related to user features
│   ├── mangas/                 # Test files related to manga features
│   ├── comments/               # Test files related to comments
├── package.json                # Project dependencies and scripts
├── .env                        # Environment variables (e.g., DB credentials)
└── README.md                   # Project documentation

```
# Endpoint Specs

## List All Mangas
Retrieve a list of all available mangas in the system.

Method: GET

URL: `/api/mangas`

Query Parameters (optional):
- `page` (integer): The page number for pagination. Defaults to 1.
- `limit` (integer): The number of mangas to return per page. Defaults to 10.

### Response
```
{
  "page": 1,
  "limit": 10,
  "totalCount": 100,
  "mangas": [
    {
      "id": "string",
      "title": "string",
      "author": "string",
      "description": "string",
      "thumbnailUrl": "string",
      "releaseDate": "string" // ISO 8601 date format
    },
    ...
  ]
}
```
# Get Manga Chapters
Retrieve all chapters for a specific manga identified by mangaId.

Method: GET

URL: `/api/mangas/:mangaId/chapters`

Path Parameters:
- `mangaId` (string): Unique identifier for the manga.

Response
```
{
  "mangaId": "string",
  "chapters": [
    {
      "chapterId": "string",
      "title": "string",
      "chapterNumber": "integer",
      "releaseDate": "string", // ISO 8601 date format
      "imageCount": "integer"
    },
    ...
  ]
}
```
# Read a Manga Chapter
Retrieve the contents of a specific chapter for a manga, including the image URLs based on user preferences.

Method: GET

URL: `/api/mangas/:mangaId/chapters/:chapterId`

Path Parameters:
- `mangaId` (string): Unique identifier for the manga.
- `chapterId` (string): Unique identifier for the chapter.

Response
```
{
  "mangaId": "string",
  "chapterId": "string",
  "title": "string",
  "chapterNumber": "integer",
  "images": [
    {
      "url": "string", // S3 URL for the image
      "resolution": "string" // e.g., "low", "high"
    },
    ...
  ],
  "totalImages": "integer"
}
```