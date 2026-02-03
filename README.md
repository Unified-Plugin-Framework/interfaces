# @unified-plugin-framework/interfaces

TypeScript interface definitions and shared contracts for the Unified Plugin Framework.

## Overview

This package contains the core interface definitions that all UPF plugins use to communicate. It provides:

- **Infrastructure Interfaces**: `IAuth`, `IStorage`, `ICache`, `IFiles`, `IMessageBus`
- **Plugin Contracts**: Types for plugin manifests, lifecycle, and configuration
- **Protobuf Definitions**: Shared `.proto` files for gRPC services
- **JSON Schemas**: Validation schemas for manifests and configuration

## Installation

```bash
npm install @unified-plugin-framework/interfaces
# or
bun add @unified-plugin-framework/interfaces
```

## Usage

```typescript
import { IStorage, ICache, IMessageBus } from '@unified-plugin-framework/interfaces';
import type { PluginManifest, PluginLifecycle } from '@unified-plugin-framework/interfaces';

// Implement an interface
class MyStoragePlugin implements IStorage {
  async get(key: string): Promise<unknown> {
    // Implementation
  }
  // ... other methods
}
```

## Core Interfaces

### IAuth

Authentication and authorization interface.

```typescript
interface IAuth {
  validateToken(token: string): Promise<TokenValidation>;
  createToken(userId: string, claims: Claims): Promise<string>;
  revokeToken(token: string): Promise<void>;
  hasPermission(userId: string, resource: string, action: string): Promise<boolean>;
}
```

### IStorage

Persistent data storage interface.

```typescript
interface IStorage {
  get<T>(key: string): Promise<T | null>;
  set<T>(key: string, value: T, options?: SetOptions): Promise<void>;
  delete(key: string): Promise<boolean>;
  query<T>(collection: string, query: Query): Promise<QueryResult<T>>;
}
```

### ICache

Fast in-memory caching interface.

```typescript
interface ICache {
  get<T>(key: string): Promise<T | null>;
  set<T>(key: string, value: T, ttl?: number): Promise<void>;
  delete(key: string): Promise<boolean>;
  invalidatePattern(pattern: string): Promise<number>;
}
```

### IFiles

File/object storage interface.

```typescript
interface IFiles {
  get(path: string): Promise<FileContent>;
  put(path: string, content: Buffer, metadata?: FileMetadata): Promise<string>;
  delete(path: string): Promise<boolean>;
  list(prefix: string): Promise<FileInfo[]>;
  getSignedUrl(path: string, expiresIn: number): Promise<string>;
}
```

### IMessageBus

Asynchronous messaging interface.

```typescript
interface IMessageBus {
  publish(topic: string, message: Message): Promise<void>;
  subscribe(topic: string, handler: MessageHandler): Promise<Subscription>;
  request<T>(topic: string, message: Message, timeout?: number): Promise<T>;
}
```

## Documentation

See the [full documentation](https://unified-plugin-framework.github.io/docs/specifications/interfaces) for complete interface definitions.

## Related Packages

- [@unified-plugin-framework/sdk](https://github.com/Unified-Plugin-Framework/sdk) - Plugin development SDK
- [@unified-plugin-framework/cli](https://github.com/Unified-Plugin-Framework/cli) - Command line tools

## License

MIT
