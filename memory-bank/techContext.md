# Technical Context: NicePhim Platform

## Architecture Overview
- **Frontend**: Next.js 15 with App Router (45 TypeScript/React files)
- **Backend**: Spring Boot 3.1.5 with Java 17 (40 Java files)
- **Database**: Microsoft SQL Server with Flyway migrations
- **Real-time**: WebSocket with STOMP protocol
- **Video Processing**: FFmpeg for HLS conversion with multiple quality variants

## Frontend Stack
- **Framework**: Next.js 15.5.2 with React 19.1.0 and App Router
- **Language**: TypeScript 5 with comprehensive type definitions
- **Styling**: Tailwind CSS 4 with glass-morphism effects and animations
- **UI Components**: Headless UI, Heroicons, custom React components
- **State Management**: React hooks with local state and API integration
- **Video Player**: React Player, HLS.js for adaptive streaming, custom SimpleHLSPlayer
- **Broadcast Scheduling UI**: Time selection interfaces with countdown displays and status indicators
- **Real-time Updates**: WebSocket integration for broadcast state synchronization
- **Image Handling**: Next.js Image component with external domain configuration

## Backend Stack
- **Framework**: Spring Boot 3.1.5 with MVC architecture
- **Language**: Java 17 with modern features
- **Database**: SQL Server with JDBC and JdbcTemplate
- **Security**: BCrypt password hashing with validation
- **Validation**: Jakarta Validation with Vietnamese error messages
- **Migration**: Flyway 9.16.0 for database schema management
- **WebSocket**: Spring WebSocket with STOMP protocol
- **Broadcast Scheduling**: Server-managed time synchronization with room state management
- **Time Calculation**: Server-side algorithms for synchronized playback position calculation
- **Architecture**: Controllers, Services, Repositories, DTOs with proper separation
- **Error Handling**: Comprehensive exception handling with HTTP responses
- **Data Access**: Manual SQL queries with JdbcTemplate for performance

## Database Schema (Simplified - V7)
- **Users**: Authentication and profile data (UUID primary key)
- **Movies**: Content metadata and relationships (includes video_id, hls_url, video_status fields)
- **Genres**: Movie categorization system (UUID primary key)
- **Movie_Genres**: Many-to-many junction table for movie-genre relationships
- **Watch Rooms**: Collaborative viewing sessions (simplified - no broadcast scheduling, no private rooms)
- **Flyway Schema History**: Migration tracking (system table)

**Removed Tables (V7 Migration):**
- ❌ Episodes (series support not implemented)
- ❌ Video_Renditions (HLS variants generated dynamically, not stored)
- ❌ Assets (original file tracking not needed)
- ❌ Comments (comment system not implemented)
- ❌ Comment_Reactions (reaction feature not implemented)
- ❌ User_Favorites (favorites feature not implemented)
- ❌ Watch_Room_Members (tracked in-memory via WatchRoomService)
- ❌ Watch_Room_Messages (chat is real-time only, not persisted)
- ❌ Watch_Room_Events (event logging not implemented)
- ❌ Watch_Room_Control_Delegations (advanced permissions not needed)

## Development Environment
- **Database**: SQL Server on localhost:1433 (Docker container)
- **Media Storage**: Local file system (D:/videos_demo, D:/media)
- **FFmpeg**: Configurable path via MEDIA_FFMPEG_PATH environment variable
- **CORS**: Configured for cross-origin requests
- **Upload Limits**: Spring Boot configured for **5GB file uploads** (10x increase from 500MB)
- **Upload Timeout**: 600 seconds (10 minutes) for large file processing
- **Video Processing**: FFmpeg creating HLS streams with 5 quality variants (4K, 2K, 1080p, 720p, 360p)
- **Video Player Libraries**: hls.js 1.6.12 for adaptive streaming, @types/hls.js for TypeScript support
- **Environment Configuration**: .env file support for flexible directory URL management without source code modifications
- **Docker**: SQL Server 2022 running in Docker container with persistent volume

## Key Dependencies
- Spring Boot Web, WebSocket, Security
- Microsoft SQL Server JDBC Driver
- Flyway for database migrations
- BCrypt for password hashing
- Next.js with TypeScript and Tailwind
- Jakarta Validation for input validation
- Spring JDBC for database operations
- hls.js for adaptive video streaming
- @types/hls.js for TypeScript support
- STOMP WebSocket client for real-time communication
- SockJS for WebSocket fallback support
- Server-side time synchronization algorithms
- Broadcast state management services

## Authentication System
- **Registration**: Complete with validation and error handling
- **Login**: Complete with both backend and frontend implementation
- **Password Security**: BCrypt hashing with salt rounds
- **User Lookup**: Supports both username and email
- **Error Handling**: Comprehensive validation with Vietnamese messages

## Movie Management System
- **MovieService**: Complete CRUD operations for movie management
- **Database Operations**: Full integration with MovieRepository
- **Error Handling**: Vietnamese error messages and proper exception handling
- **Validation**: Input validation for all movie operations
- **Response Mapping**: Proper DTO conversion for API responses

## Genre Management System
- **GenreController**: REST API endpoints for genre CRUD operations
- **GenreService**: Business logic layer with validation and error handling
- **GenreRepository**: Database operations with JdbcTemplate
- **Genre Model**: UUID-based entity with name field
- **DTOs**: CreateGenreRequest, UpdateGenreRequest, GenreResponse
- **Frontend Integration**: Complete admin interface with create, read, update, delete
- **Error Handling**: Enhanced frontend error handling with warning vs error classification
- **Validation**: Server-side validation with Vietnamese error messages

## Development Environment & Standards
- **Database Migration**: Flyway automatic migration execution during Spring Boot startup
- **Migration Management**: V2 migration successfully applied with video fields (video_id, hls_url, video_status)
- **Error Resolution**: Temporary feature disabling during migration issues, then re-enabling
- **Cursor Rules**: Comprehensive development guidelines and standards
- **Memory Bank**: Complete documentation system with hierarchical structure
- **Code Quality**: TypeScript interfaces, Java best practices, database optimization
- **Documentation Workflow**: Memory bank updates triggered by significant changes
- **Development Guidelines**: Complete cursor rules integration with memory bank system
- **Error Handling**: Warning vs error classification, auto-clear functionality
- **Security Standards**: BCrypt hashing, input validation, SQL injection prevention
- **Performance Standards**: Efficient queries, caching, parallel API calls


