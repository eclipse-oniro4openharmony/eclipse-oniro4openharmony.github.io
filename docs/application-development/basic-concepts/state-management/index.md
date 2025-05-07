# State Management

This section covers the key concepts, decorators, and storage mechanisms used for managing state in ArkTS/ArkUI applications.

State management in ArkTS/ArkUI enables you to build dynamic, interactive UIs by keeping your application’s data and UI in sync. This documentation explains the main concepts, decorators, and storage solutions available for handling state at different scopes and lifecycles.

## Key Topics Covered

- **`@State` Decorator**  
  Declares reactive local state within a component. Changes to `@State` variables automatically trigger UI updates.

- **Other Decorators**  
  Enable one-way or two-way data binding, state sharing between components, and listening for state changes:
  - `@Prop`
  - `@Link`
  - `@ObjectLink`
  - `@StorageProp`
  - `@StorageLink`
  - `@LocalStorageProp`
  - `@LocalStorageLink`
  - `@Provide`
  - `@Consume`
  - `@Watch`

- **AppStorage**  
  A global, application-wide storage for sharing state across all components and pages. Supports decorators for automatic UI synchronization.

- **LocalStorage**  
  Provides in-memory storage for sharing state between components or pages within a limited scope. Offers decorators for one-way and two-way binding.

- **PersistentStorage**  
  Persists selected AppStorage attributes to device storage, ensuring state is retained across app restarts.

- **`$$` Operator**  
  Enables two-way synchronization between TypeScript variables and built-in component state.

- **State Management Patterns**  
  Examples and scenarios for using the above mechanisms in real applications.
