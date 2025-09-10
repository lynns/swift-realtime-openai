# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Swift Package Manager (SPM) library called "OpenAIRealtime" that provides a modern Swift SDK for OpenAI's Realtime API. It enables multi-modal conversations with automatic voice handling and supports both WebSocket and WebRTC connections.

## Development Commands

### Building
```bash
# Standard SPM build
swift build

# Create XCFramework for distribution (as used in CI)
./.github/build.sh
```

### Testing
No formal test suite is currently configured. The project relies on manual testing and CI builds.

### Package Management
```bash
# Resolve dependencies
swift package resolve

# Update dependencies  
swift package update

# Generate Xcode project (if needed)
swift package generate-xcodeproj
```

## Architecture

### Core Components

- **`RealtimeAPI`** (`src/OpenAIRealtime.swift`): Low-level API client that abstracts connection protocols
- **`Conversation`** (`src/Conversation.swift`): High-level interface managing conversation state, audio handling, and user interactions
- **Connectors** (`src/Connectors/`): Protocol-based connection implementations
  - `WebSocketConnector`: WebSocket-based connection to OpenAI
  - `WebRTCConnector`: WebRTC-based connection (experimental)
- **Models** (`src/Models/`): Data structures for API communication
  - `Session`: Session configuration and state
  - `Item`: Conversation items (messages, audio, etc.)  
  - `ClientEvent`/`ServerEvent`: API event types
  - `Response`: API response configurations

### Key Design Patterns

- **Protocol-oriented**: `Connector` protocol allows pluggable connection types
- **Swift Concurrency**: Extensive use of async/await and AsyncStream
- **Observable**: `Conversation` class uses `@Observable` for SwiftUI integration
- **Audio Engine**: Uses `AVAudioEngine` for real-time audio processing

### Directory Structure

```
src/
├── OpenAIRealtime.swift     # Main API client
├── Conversation.swift       # High-level conversation manager  
├── Connectors/             # Connection implementations
├── Models/                 # API data structures
├── Protocols/              # Protocol definitions
├── Extensions/             # Utility extensions
└── Support/                # Thread-safe utilities
```

## Platform Support

- iOS 17.0+
- tvOS 17.0+ 
- macOS 14.0+
- watchOS 10.0+
- visionOS 1.0+
- Mac Catalyst 17.0+

## Dependencies

- **WebRTC**: External dependency for WebRTC connectivity (`https://github.com/stasel/WebRTC.git`)

## Build Configuration

The package uses a custom source path (`./src`) instead of the standard SPM `Sources/` directory. The CI build process creates dynamic frameworks and XCFrameworks for distribution.

## Key Files to Understand

- `src/Conversation.swift:1-50`: Main conversation management interface
- `src/OpenAIRealtime.swift:35-67`: API connection factory methods  
- `src/Protocols/Connector.swift:6-15`: Core connection protocol
- `Package.swift:5-24`: Package configuration and dependencies