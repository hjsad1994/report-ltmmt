# System Patterns: NicePhim Architecture

## Overall Architecture
```
Frontend (Next.js) ←→ Backend (Spring Boot) ←→ Database (SQL Server)
                           ↕
                    WebSocket (STOMP)
                           ↕
                    Video Processing (FFmpeg)
```

## Codebase Analysis
- **Backend**: 40 Java files implementing complete Spring Boot MVC architecture
- **Frontend**: 45 TypeScript/React files with Next.js App Router and modern UI components
- **Database**: SQL Server with 6 essential tables (simplified from 15) and UUID-based primary keys
- **Video Processing**: FFmpeg HLS conversion with 5 quality variants (4K, 2K, 1080p, 720p, 360p)

## Frontend Patterns
- **App Router**: Next.js 15 App Router for routing with dynamic params
- **Component Architecture**: Reusable UI components with TypeScript props
- **Type Safety**: TypeScript interfaces for all data models (Movie, Genre, User)
- **Responsive Design**: Mobile-first with Tailwind CSS and glass-morphism effects
- **State Management**: React hooks with local state and API integration
- **Cinematic UI**: Dark theme with gradients, animations, and hover effects
- **Video Player Architecture**: SimpleHLSPlayer with HLS.js adaptive streaming
- **Image Handling**: Next.js Image component with domain configuration
- **Layout Adjustments**: Negative margin utilities (-ml-*, -mt-*) for precise component positioning
- **Typography Scaling**: Responsive text sizing with consistent breakpoints (text-4xl, lg:text-6xl, etc.)
- **Authentication Management**: localStorage-based session management with automatic username detection from authenticated user accounts
- **Event-Driven Updates**: Custom 'auth-change' events for real-time username updates across components
- **Username Resolution**: Automatic UUID-to-username resolution with backend API integration and caching
- **Host Detection System**: UUID-based comparison between room creator and current user for automatic host assignment
- **Video Seeking and Sync**: Enhanced seeking functionality allowing viewers to seek video and synchronize with room creator
- **User ID Mapping**: Proper mapping between backend UUID and frontend user identification systems

## Backend Patterns
- **MVC Architecture**: Controllers, Services, Repositories, DTOs with proper separation
- **Dependency Injection**: Spring IoC container with constructor injection
- **Data Access**: JdbcTemplate for SQL operations with manual SQL queries
- **Security**: BCrypt password hashing with proper validation
- **Validation**: Jakarta Bean Validation with Vietnamese error messages
- **Error Handling**: Comprehensive exception handling with proper HTTP responses
- **UUID Architecture**: All entities use UUID primary keys for better security
- **Service Layer**: Business logic separated from data access and presentation
- **Username Resolution**: JOIN operations between watch_rooms and users tables to include creator_username in all room responses
- **Authentication Services**: Username lookup endpoints for UUID-to-username resolution with proper error handling
- **Production-Only Controllers**: Removed demo/test controllers (HelloController, DemoController) for production-ready endpoints
- **Simplified Room Creation**: Rooms always start immediately - removed complex scheduledStartTime logic
- **WebSocket-Only Communication**: Removed SSE-based RoomSSEController in favor of pure WebSocket pattern
- **Video Bitrate Optimization**: Automatic bitrate adjustment for optimal streaming quality

## Database Patterns
- **Simplified Schema**: Only 6 essential tables after V7 cleanup (users, movies, genres, movie_genres, watch_rooms, flyway_schema_history)
- **Removed Tables**: Eliminated 10 unused tables (episodes, video_renditions, assets, comments, comment_reactions, user_favorites, watch_room_members, watch_room_messages, watch_room_events, watch_room_control_delegations)
- **UUID Primary Keys**: Unique identifiers for all entities (movies, users, genres, rooms)
- **Foreign Key Constraints**: Proper referential integrity with cascading updates
- **Audit Fields**: created_at, updated_at timestamps with proper timezone handling
- **Video Integration**: Movies table includes video_id, hls_url, video_status fields
- **Junction Tables**: Many-to-many relationships (movie_genres) for flexible categorization
- **Manual SQL**: Custom SQL queries with JdbcTemplate for optimal performance
- **Migration Management**: Flyway migrations V1-V7 for schema versioning and cleanup
- **SQL Server Optimization**: Proper indexing and query optimization for large datasets
- **In-Memory Tracking**: Room members tracked in WatchRoomService.roomUsers Map instead of database

## Real-time Communication
- **WebSocket-Only Architecture**: Pure STOMP protocol for watch-together features
- **Event Broadcasting**: Real-time updates to connected clients via WebSocket
- **Room Management**: User sessions tracked in-memory (WatchRoomService.roomUsers Map)
- **Immediate Room Start**: Rooms always start immediately - no scheduling complexity
- **Public Rooms**: All rooms are public after V5 migration (is_private removed)
- **Chat Messages**: Real-time only via WebSocket - not persisted to database
- **User Freedom Model**: Individual user control over video playback without forced synchronization
- **State Management**: Simplified WebSocket message handling in watch_rooms table
- **Video Player Liberation**: Full seeking functionality and native video controls enabled
- **Broadcast Fields Removed**: No scheduledStartTime, broadcast_status, or server_managed_time after V4 migration

## Video Processing Pipeline
1. **Upload**: Original video files stored locally (D:/videos_demo) with 5GB limit and 600-second timeout
2. **Processing**: FFmpeg converts to HLS format with 5 quality variants (4K/2160p, 2K/1440p, 1080p, 720p, 360p)
3. **Dynamic Generation**: Quality variants generated based on input resolution (no upscaling)
4. **Bitrate Optimization**: Automatic bitrate adjustment for optimal streaming quality
5. **Storage**: HLS segments stored in media directory (D:/media) with variant subfolders (v0-v4)
6. **Streaming**: Direct file serving via Spring static resources
7. **Database Integration**: Only video_id, hls_url, video_status stored in movies table (no video_renditions table)
8. **Environment Configuration**: Directory paths configurable via .env file with fallback values in application.properties

## Video Player Architecture
- **SimpleHLSPlayer Component**: Simplified video player built from scratch for better maintainability
- **HLS.js Integration**: Adaptive video streaming with multiple quality options
- **Event Handling**: Proper click (play/pause) and double-click (fullscreen) functionality
- **Control Layout**: User-requested layout with timeline, seek buttons, volume, quality, and fullscreen controls
- **State Management**: Controls timeout, volume state, and fullscreen state management
- **CSS Optimization**: Pointer events management and z-index handling for proper layering
- **Event Conflict Resolution**: stopPropagation to prevent conflicts between video element and controls overlay
- **Quality/Speed Selection**: Working dropdown menus with proper state updates and click outside handling
- **Menu Management**: Data attributes and improved event handling for reliable menu interactions
- **HLS Adaptive Quality Switching**: Real-time quality switching using hls.currentLevel API with level mapping and event handling
- **Dynamic Quality Menu**: Shows only available qualities from HLS manifest with actual resolution display
- **Component Consolidation**: Single SimpleHLSPlayer component replaces legacy HLSVideoPlayer for cleaner architecture
- **Video Player Layout Optimization**: Responsive sizing with max-w-full width and optimized padding for screen utilization
- **Header Spacing Management**: Proper top padding (pt-12) applied at page level to prevent header overlap
- **Padding Configuration**: Horizontal padding (px-5) for 20px side margins and minimal vertical padding for maximum screen usage
- **Single Theme Architecture**: Simplified dark theme only, no theme switching complexity
- **Production-Only Codebase**: Clean codebase with only production features, no test files or development utilities

## Security Patterns
- **Password Hashing**: BCrypt with salt rounds
- **Input Validation**: Server-side validation for all inputs
- **CORS Configuration**: Controlled cross-origin access
- **SQL Injection Prevention**: Parameterized queries

## Admin Management Patterns
- **CRUD Operations**: Complete Create, Read, Update, Delete for all entities
- **Validation Layers**: Frontend client-side + backend server-side validation
- **Error Classification**: Warning vs Error distinction for better UX
- **Real-time Feedback**: Auto-clear errors when user starts typing
- **Consistent UI**: ## Genre-Movie Integration Patterns
- **Many-to-Many Relationships**: Proper junction table (movie_genres) design
- **Embedded Data**: MovieResponse includes genres to reduce API calls
- **Hybrid Loading**: Frontend uses embedded data with fallback to separate API calls
- **Visual Indicators**: Genre tags with icons and color coding in admin interface
- **Bulk Operations**: Support for assigning multiple genres during movie creation/editing
- **Debug Logging**: Console logging for tracking data flow between backend and frontend

## Development Standards Patterns
- **Cursor Rules Integration**: Comprehensive development guidelines documented in .cursorrules
- **Memory Bank Documentation**: Complete documentation system with hierarchical structure
- **Code Quality Standards**: TypeScript interfaces, Java best practices, database optimization
- **Error Handling Patterns**: Warning vs error classification, auto-clear functionality
- **Security Patterns**: BCrypt hashing, input validation, SQL injection prevention
- **Performance Patterns**: Efficient queries, caching, parallel API calls
- **Testing Patterns**: Comprehensive validation, responsive design testing
- **File Organization**: Standardized frontend and backend structures
- **Documentation Workflow**: Memory bank updates triggered by significant changes or user requests
- **Development Guidelines**: Complete cursor rules integration with memory bank system
- **Debug Logging**: Console logging for tracking data flow between backend and frontend
- **Environment Configuration**: .env file support for flexible configuration management without source code modifications

## Database Migration Patterns
- **Flyway Integration**: Automatic database migration execution during Spring Boot startup
- **Migration Versioning**: Sequential version numbering (V1, V2, V3) for database schema changes
- **Backward Compatibility**: Temporary feature disabling during migration issues
- **Error Resolution Strategy**: Fix repository layer first, then re-enable features
- **Schema Evolution**: Add new columns with NULL defaults to avoid breaking existing data
- **Migration Rollback**: Proper migration design to allow rollback if needed
- **Column Addition Pattern**: ALTER TABLE ADD COLUMN with appropriate defaults and constraints

## Room Creation Patterns (Simplified)
- **Immediate Start Architecture**: Rooms always start immediately when created - no scheduling logic
- **Simplified Room Creation**: Removed scheduledStartTime parameters and complex broadcast scheduling
- **User Freedom Pattern**: Individual control over video playback without forced synchronization
- **Simplified State Management**: Removal of complex sync state variables and periodic polling
- **WebSocket-Only Communication**: Pure WebSocket pattern without SSE complexity
- **Performance Optimization**: Reduced logging and eliminated spammy sync messages
- **Video Player Liberation**: Full seeking functionality and native video controls enabled
- **Removed Complexity**: Eliminated BroadcastSchedulerService, automatic broadcast start checking, and scheduled broadcast management
- **Streamlined Backend**: Deleted RoomSSEController and simplified BroadcastScheduler
- **Clean Architecture**: Production-ready endpoints without demo/test controllers

## Database Integration Patterns
- **Type Safety Pattern**: Proper handling of Java type mappings for SQL Server data types (TINYINT → Short, VARCHAR → String)
- **Unique Constraint Management**: Generation of unique identifiers for fields with UNIQUE constraints to avoid null value violations
- **Cross-Origin Resource Sharing**: CORS configuration for all controllers that need to handle frontend API requests
- **Password Hashing Pattern**: Consistent use of BCrypt for all password operations with proper salt generation
- **UUID Validation**: Frontend validation of UUID format before sending to backend to prevent conversion errors
- **Database Logging**: Comprehensive logging for database operations to facilitate debugging and monitoring

## Project Structure Patterns
- **Frontend Directory**: nicephim-frontend (renamed from rophim-frontend)
- **Backend Directory**: nicephim-backend
- **Consistent Naming**: All project references use "NicePhim" branding
- **Memory Bank Integration**: All documentation files updated with new branding
- **Code References**: Updated all code references from rophim to nicephim


