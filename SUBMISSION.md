# Submission Notes

## Codebase Map

This project is a Flask + SQLAlchemy app for a social music experience. The app is organized around a small set of route modules, service modules, and shared database models.

### Main files and their roles

- app.py
  - Creates the Flask application with the app factory pattern.
  - Configures the database connection, initializes SQLAlchemy, registers all blueprints, and creates the database tables.

- models.py
  - Defines the core database schema.
  - Contains the main entities: User, Song, ListeningEvent, Rating, Playlist, Notification, plus the association tables for friendships, song tags, and playlist entries.

- routes/songs.py
  - Exposes endpoints for searching songs, viewing song details, rating songs, and recording listens.

- routes/playlists.py
  - Exposes endpoints for creating playlists, viewing playlist details, listing playlist songs, and adding songs to playlists.

- routes/users.py
  - Exposes endpoints for user profile lookup, streak lookup, notification retrieval, and marking notifications as read.

- routes/feed.py
  - Exposes endpoints for the "Friends Listening Now" feed and the general activity feed.

- services/search_service.py
  - Implements the search logic for songs by title and artist.

- services/notification_service.py
  - Creates notifications and handles notification-producing actions such as rating a song and adding a song to a playlist.

- services/streak_service.py
  - Contains the business logic for updating and reading a user's listening streak.

- services/feed_service.py
  - Builds the recent-friends and activity-feed responses from listening events.

- services/playlist_service.py
  - Handles playlist creation and retrieval of songs in playlist order.

- seed_data.py
  - Populates the database with sample users, songs, playlists, and related records for local testing.

- tests/
  - Contains regression tests for playlist behavior, search behavior, and streak logic.

### Architectural flow

The application follows a simple pattern:

1. A route receives an HTTP request.
2. The route calls a service function in the services layer.
3. The service reads or writes through SQLAlchemy models.
4. The route returns a JSON response to the client.

### Example data flow: song search

- A client sends a request to /songs/search?q=... .
- The route in routes/songs.py reads the query string and calls search_songs(query).
- The service in services/search_service.py queries the Song table, filters by title or artist, and serializes the matching songs into dictionaries.
- The route returns the results as JSON, which the client can render.

### Notable implementation notes

- The app uses Flask blueprints to separate feature areas.
- Business logic is concentrated in the services layer rather than inside the routes.
- The database models are the shared contract between routes, services, and tests.
