# Coding Guide for Development Sequence

This coding guide outlines the step-by-step process for building the AI-assisted writing application. The guide is designed to help developers lay the groundwork first before building on top of it, ensuring that all interconnected dependencies are properly managed. Each step references the corresponding sections in the techspec document to provide context and detailed requirements.

---

## Table of Contents

1. [Set Up Development Environment](#1-set-up-development-environment)
2. [Initialize the Project](#2-initialize-the-project)
3. [Set Up Electron Main Process](#3-set-up-electron-main-process)
4. [Create Preload Script and Set Up IPC Communication](#4-create-preload-script-and-set-up-ipc-communication)
5. [Set Up Basic React Application](#5-set-up-basic-react-application)
6. [Implement State Management with Zustand](#6-implement-state-management-with-zustand)
7. [Implement Error Boundaries in React Components](#7-implement-error-boundaries-in-react-components)
8. [Create UI Layout Structure](#8-create-ui-layout-structure)
9. [Implement Left Panel Functionality](#9-implement-left-panel-functionality)
10. [Implement Middle Panel Functionality](#10-implement-middle-panel-functionality)
11. [Implement Right Panel Functionality](#11-implement-right-panel-functionality)
12. [Implement Panel State Persistence](#12-implement-panel-state-persistence)
13. [Implement Configuration Management System](#13-implement-configuration-management-system)
14. [Implement File System Services](#14-implement-file-system-services)
15. [Implement OpenAI Integration](#15-implement-openai-integration)
16. [Implement Content Combiner Service](#16-implement-content-combiner-service)
17. [Implement Comprehensive Error Handling](#17-implement-comprehensive-error-handling)
18. [Implement Testing Strategy](#18-implement-testing-strategy)
19. [Optimize Performance](#19-optimize-performance)
20. [Ensure Accessibility Compliance](#20-ensure-accessibility-compliance)
21. [Develop Documentation](#21-develop-documentation)
22. [Set Up Continuous Integration and Code Coverage](#22-set-up-continuous-integration-and-code-coverage)

---

## 1. Set Up Development Environment

**Reference:** [1.1. Environment Setup Requirements](#11-environment-setup-requirements)

### 1.1. Install Development Tools

- **Node.js and npm/yarn**: Install the latest LTS version of Node.js from the [official website](https://nodejs.org/). Verify the installation with `node -v` and `npm -v`.
- **Yarn**: Install globally using npm with `npm install -g yarn`. Verify with `yarn -v`.
- **TypeScript Compiler**: Install globally using `npm install -g typescript`. Verify with `tsc -v`.
- **Visual Studio Code (VSCode)**: Download and install from the [VSCode website](https://code.visualstudio.com/). Install recommended extensions:
  - **ESLint**
  - **Prettier**
  - **TypeScript Hero**
- **Git**: Install the latest version and configure user information:
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "your.email@example.com"
  ```
- **Disk Space**: Ensure at least 2GB of free disk space.

### 1.2. Verify System Requirements

- **Operating System Compatibility**: Ensure compatibility with Windows 10+, macOS Catalina+, or a modern Linux distribution.
- **Build Tools for Native Modules**:
  - **Windows**: Install [Visual Studio Build Tools](https://visualstudio.microsoft.com/downloads/).
  - **macOS**: Install Xcode Command Line Tools with `xcode-select --install`.
  - **Linux**: Install build-essential with `sudo apt-get install build-essential`.

---

## 2. Initialize the Project

**Reference:** [1.2. Project Creation Architecture](#12-project-creation-architecture)

### 2.1. Create Base Project Structure

Organize the root directory as follows:

- `/src` - Source code
- `/config` - Configuration files
- `/assets` - Static resources
- `/build` - Build output
- `/types` - TypeScript type definitions

### 2.2. Initialize Git Repository

- Navigate to the project root directory and run `git init`.
- Add a `.gitignore` file to exclude `node_modules`, `build`, and other unnecessary files.

### 2.3. Initialize `package.json`

- Run `yarn init -y` to create a default `package.json`.
- Update the `package.json` with project details and scripts.

### 2.4. Set Up Configuration Files

#### 2.4.1. TypeScript Configuration

Create `tsconfig.json` with strict type checking and appropriate module resolution settings.

#### 2.4.2. Electron Configuration

Set up Electron-specific configurations, including main and renderer process settings, IPC communication setup, and security policies.

---

## 3. Set Up Electron Main Process

**Reference:** [4.1. Set Up Electron Main Process](#41-set-up-electron-main-process)

### 3.1. Create Main Script

- In the `/src` directory, create `main.ts` as the entry point of the application.

### 3.2. Initialize the App

- Import Electron's `app` module and handle the application lifecycle events.
- Ensure the app quits when all windows are closed.

### 3.3. Create a Browser Window

- Use Electron's `BrowserWindow` class to create the main application window.
- Configure window properties such as dimensions, title bar style, and security settings.

### 3.4. Load Initial Content

- Use `loadFile` or `loadURL` to load the initial HTML file into the window.
- Ensure the renderer process is set up to handle React rendering.

---

## 4. Create Preload Script and Set Up IPC Communication

**References:**

- [4.2. Create Preload Script](#42-create-preload-script)
- [4.3. Set Up IPC Communication](#43-set-up-ipc-communication)

### 4.1. Create Preload Script

- In the `/src` directory, create `preload.ts`.
- Use Electron's `contextBridge` to expose necessary APIs to the renderer process securely.
- Enable `contextIsolation` in the `webPreferences` of `BrowserWindow`.

### 4.2. Set Up IPC Communication

- In `main.ts`, use `ipcMain` to handle events from the renderer process.
- In `preload.ts`, expose `ipcRenderer` methods to the renderer process.
- Define communication channels for different message types.
- Validate and sanitize all data received through IPC for security.

---

## 5. Set Up Basic React Application

**Reference:** [3.1.2. Overall UI Architecture](#312-overall-ui-architecture)

### 5.1. Install React and Dependencies

- Install React, ReactDOM, and necessary TypeScript types:
  ```bash
  yarn add react react-dom
  yarn add -D @types/react @types/react-dom
  ```

### 5.2. Create Main React Component

- In the `/src` directory, create `App.tsx` as the root React component.
- Set up a basic component structure to render "Hello, World!" as a test.

### 5.3. Configure Webpack or Vite

- Set up a build tool (Webpack or Vite) to bundle the renderer process code.
- Ensure that the build tool is configured to handle TypeScript and React.

### 5.4. Integrate React with Electron

- In `index.html`, include a root element for React to render into.
- In the renderer process entry point, render the `App` component into the DOM.

---

## 6. Implement State Management with Zustand

**Reference:** [3.1.2.4. State Management](#3124-state-management)

### 6.1. Install Zustand and Middleware

- Install Zustand and `zustand-persist`:
  ```bash
  yarn add zustand zustand-persist
  ```

### 6.2. Create Global Store

- In the `/src` directory, create `store.ts`.
- Define the application state interface, including window state, panel states, editor state, AI assistant state, and user preferences.
- Implement the store using `create` and `persist` from Zustand.

**Example:**

```typescript
import create from 'zustand';
import { persist } from 'zustand/middleware';

interface AppState {
  window: WindowState;
  panels: PanelsState;
  // ... other states
  setWindowState: (state: WindowState) => void;
  // ... other actions
}

export const useStore = create<AppState>()(
  persist(
    (set, get) => ({
      // Initial state and actions
    }),
    {
      name: 'app-state',
      getStorage: () => window.localStorage,
    }
  )
);
```

---

## 7. Implement Error Boundaries in React Components

**Reference:** [3.1.2.5. Error Boundaries in React Components](#3125-error-boundaries-in-react-components)

### 7.1. Install `react-error-boundary`

- Install the library:
  ```bash
  yarn add react-error-boundary
  ```

### 7.2. Create Error Boundary Component

- In `/src/components`, create `ErrorBoundary.tsx`.
- Implement a fallback UI to display when an error occurs.

**Example:**

```tsx
import React from 'react';
import { ErrorBoundary as ReactErrorBoundary } from 'react-error-boundary';

function FallbackComponent({ error, resetErrorBoundary }) {
  return (
    <div role="alert">
      <p>An error occurred: {error.message}</p>
      <button onClick={resetErrorBoundary}>Try Again</button>
    </div>
  );
}

export function ErrorBoundary({ children }) {
  return (
    <ReactErrorBoundary FallbackComponent={FallbackComponent}>
      {children}
    </ReactErrorBoundary>
  );
}
```

### 7.3. Wrap App Component

- In `App.tsx`, wrap the main content with `ErrorBoundary`.

---

## 8. Create UI Layout Structure

**References:**

- [3.1.2.2. Layout Structure](#3122-layout-structure)
- [3.1.2.3. Panel-Specific Requirements](#3123-panel-specific-requirements)

### 8.1. Implement Main Layout

- Use CSS Grid or Flexbox to create a three-panel layout.
- In `/src/components`, create `MainLayout.tsx`.

### 8.2. Create Panel Components

- Create `LeftPanel.tsx`, `MiddlePanel.tsx`, and `RightPanel.tsx` in `/src/components`.

### 8.3. Implement Resizable Panels

- Install `react-resizable`:
  ```bash
  yarn add react-resizable
  ```
- Implement resizable functionality in the panel components.

---

## 9. Implement Left Panel Functionality

**Reference:** [3.1.2.3. Panel-Specific Requirements > Left Panel Implementation](#left-panel-implementation)

### 9.1. Implement Tab System

- Use Material-UI (MUI) components for tabs.
- Install MUI:
  ```bash
  yarn add @mui/material @mui/icons-material @emotion/react @emotion/styled
  ```

### 9.2. Implement Writing Prompts Tab

- Create components for folder selection, category selection, and prompt display.
- Use Electron's `dialog` module via the preload script for folder selection.

### 9.3. Implement AI Assistant Tab

- Create a chat interface resembling iOS Messenger.
- Manage conversation history using Zustand.

---

## 10. Implement Middle Panel Functionality

**Reference:** [3.1.2.3. Panel-Specific Requirements > Middle Panel Implementation](#middle-panel-implementation)

### 10.1. Implement Text Editor Component

- Install `react-simple-code-editor` or use `@monaco-editor/react`:
  ```bash
  yarn add react-simple-code-editor
  ```
- Implement the editor with features like word count, auto-save, and undo/redo.

### 10.2. Implement Action Buttons

- Create `Submit for Feedback` and `Clear` buttons with appropriate event handlers.
- Use the content combiner service to prepare submissions.

---

## 11. Implement Right Panel Functionality

**Reference:** [3.1.2.3. Panel-Specific Requirements > Right Panel Implementation](#right-panel-implementation)

### 11.1. Implement Tab System

- Use MUI components to create tabs for `AI Feedback`, `Select Criteria`, and `Settings`.

### 11.2. Implement AI Feedback Tab

- Install `react-markdown` to display AI feedback in markdown format:
  ```bash
  yarn add react-markdown
  ```

### 11.3. Implement Select Criteria Tab

- Create components for folder and file selection to choose evaluation criteria.

### 11.4. Implement Settings Tab

- Provide interfaces for adjusting application settings and storing them using the configuration management system.

---

## 12. Implement Panel State Persistence

**References:**

- [3.1.2.4. State Management](#3124-state-management)
- [8.2. Panel Persistence](#82-panel-persistence)

### 12.1. Persist Panel States

- Use the Zustand store to manage and persist panel states such as sizes and active tabs.
- Ensure that state changes are saved to `localStorage` or `electron-store`.

---

## 13. Implement Configuration Management System

**Reference:** [2. Configuration Architecture Analysis](#2-architecture-analysis)

### 13.1. Create Configuration Files

- In `/config`, create configuration files such as `appConfig.ts`, `panelConfig.ts`, `fileSystemConfig.ts`, `openAIConfig.ts`, and `uiConfig.ts`.

### 13.2. Implement Configuration Service

- Create a service that loads configurations during app startup and provides access to configuration values throughout the app.
- Implement runtime configuration changes and persistence of user preferences.

---

## 14. Implement File System Services

**References:**

- [3.2.1. OS Manipulation Implementation](#321-os-manipulation-implementation)
- [3.2.2. File Manipulation Implementation](#322-file-manipulation-implementation)

### 14.1. Use Electron's File System Modules

- Use `fs` and `dialog` modules via the preload script to handle file operations securely.

### 14.2. Implement File System Service

- Create `FileSystemService.ts` to handle file reading, writing, and watching.
- Ensure asynchronous operations are handled with `async/await`.

---

## 15. Implement OpenAI Integration

**Reference:** [3.2.3. OpenAI Integration Implementation](#323-openai-integration-implementation)

### 15.1. Securely Store API Key

- Install `keytar` for secure credential storage:
  ```bash
  yarn add keytar
  ```
- Implement API key management in the settings tab and store it securely.

### 15.2. Implement OpenAI Service

- Install OpenAI's Node.js library:
  ```bash
  yarn add openai
  ```
- Create `OpenAIService.ts` to handle API calls.
- Manage conversation context for the AI Assistant.

---

## 16. Implement Content Combiner Service

**Reference:** [8.1. Content Combiner](#81-content-combiner)

### 16.1. Create `contentCombinerConfig.ts`

- Define configuration for combining content, including prefixes, suffixes, and validation rules.

### 16.2. Implement `ContentCombinerService.ts`

- Implement methods to validate and combine content from the prompt, submission, and criteria.
- Ensure content adheres to OpenAI's token limits.

---

## 17. Implement Comprehensive Error Handling

**References:**

- [3.6. Error Handling Requirements](#36-error-handling-requirements)
- [5.4. Error Handling Strategy](#54-error-handling-strategy)

### 17.1. Implement Error Handling in Services

- Use `try/catch` blocks around asynchronous operations.
- Throw custom error classes for specific error types.

### 17.2. Implement User-Friendly Error Messages

- Provide clear feedback to users when errors occur.
- Use the error boundary component to display fallback UI in the event of rendering errors.

---

## 18. Implement Testing Strategy

**References:**

- [3.7. Testing Requirements](#37-testing-requirements)
- [6. Testing Strategy](#6-testing-strategy)

### 18.1. Set Up Testing Framework

- Install Jest and related testing libraries:
  ```bash
  yarn add -D jest @types/jest ts-jest @testing-library/react @testing-library/jest-dom
  ```
- Configure Jest for TypeScript and React.

### 18.2. Write Unit Tests

- Write unit tests for all services and utility functions.
- Use mocks and spies as needed.

### 18.3. Write Integration Tests

- Use `nock` to mock HTTP requests to the OpenAI API.
- Use `jest.mock()` to mock modules like `fs`.

### 18.4. Write End-to-End Tests

- Install and set up Spectron for Electron end-to-end testing:
  ```bash
  yarn add -D spectron
  ```
- Write tests to simulate user interactions and verify application behavior.

---

## 19. Optimize Performance

**References:**

- [3.5. Performance Considerations](#35-performance-considerations)
- [5.5. Performance Considerations](#55-performance-considerations)

### 19.1. Optimize React Rendering

- Use `React.memo` to prevent unnecessary re-renders of components.
- Use `useCallback` and `useMemo` hooks to optimize functions and computed values.

### 19.2. Efficient State Management

- Ensure that state updates in Zustand are minimal and scoped to necessary components.

---

## 20. Ensure Accessibility Compliance

**References:**

- [3.1.2.9. Accessibility Requirements](#3129-accessibility-requirements)
- [3.9. Accessibility Requirements](#39-accessibility-requirements)

### 20.1. Implement Semantic HTML

- Use appropriate HTML elements and ARIA attributes in React components.

### 20.2. Keyboard Navigation

- Ensure all interactive elements are accessible via keyboard.

### 20.3. Contrast and Readability

- Use color schemes that meet WCAG 2.1 AA standards for contrast.

---

## 21. Develop Documentation

**References:**

- [1.4.2. Documentation Requirements](#142-documentation-requirements)
- [5.3. Documentation System](#53-documentation-system)

### 21.1. Code Documentation

- Use JSDoc comments in code to describe functions, classes, and modules.
- Generate API documentation using tools like TypeDoc.

### 21.2. Project Documentation

- Create a `docs/` directory.
- Include setup instructions, development guidelines, and troubleshooting guides.

---

## 22. Set Up Continuous Integration and Code Coverage

**References:**

- [6.3. Code Coverage Requirements](#63-code-coverage-requirements)

### 22.1. Configure Code Coverage

- Use Jest's coverage feature to measure code coverage.
- Ensure coverage thresholds are met:
  - Statements: 85%
  - Branches: 80%
  - Functions: 85%
  - Lines: 85%

### 22.2. Integrate with CI/CD Pipeline

- Create GitHub Actions workflows to run tests and report coverage.
- Use `coveralls` to report coverage to a coverage service.

**Example Workflow (`.github/workflows/test.yml`):**

```yaml
name: Test and Coverage

on: [push, pull_request]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - name: Install dependencies
        run: yarn install --frozen-lockfile
      - name: Run Tests with Coverage
        run: yarn test:coverage
      - name: Upload Coverage to Coveralls
        uses: coverallsapp/github-action@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

---

## Table of Contents

1. [Project Overview]
    - [Environment Setup Requirements]
    - [Project Creation Architecture]
    - [Logical Flow]
    - [Development Workflow]
    - [UI Library and Component Ecosystem]
2. [Architecture Analysis]
    - [Configuration Architecture Analysis]
    - [Configuration Management System]
    - [Type System]
    - [Configuration Access Pattern]
    - [Integration Points]
    - [Security Considerations]
    - [Error Handling]
    - [Testing Strategy]
3. [Application Design and Implementation]
    - [Front-end/UI Design]
        - [Application Frame Implementation Plan]
        - [Overall UI Architecture]
            - [Layout Structure]
            - [Panel-Specific Requirements]
                - [Left Panel Implementation]
                - [Middle Panel Implementation]
                - [Right Panel Implementation]
            - [State Management]
            - [Error Boundaries in React Components]
            - [Event Handling]
            - [Cross-Panel Communication]
            - [Styling Requirements]
            - [Accessibility Requirements]
    - [Back-end Features]
        - [OS Manipulation Implementation]
        - [File Manipulation Implementation]
        - [OpenAI Integration Implementation]
    - [Implementation Flow]
    - [Cross-Platform Compatibility]
    - [Performance Considerations]
    - [Error Handling Requirements]
    - [Testing Requirements]
    - [Documentation Needs]
    - [Accessibility Requirements]
    - [Conclusion]
4. [Implementation Details]
    - [Set Up Electron Main Process]
    - [Create Preload Script]
    - [Set Up IPC Communication]
5. [Best Practices]
    - [Component Registration System]
    - [Type-First Development]
    - [Documentation System]
    - [Error Handling Strategy]
    - [Performance Considerations]
    - [Security Measures]
    - [Testing Strategy Overview]
    - [Error Monitoring and Reporting]
6. [Testing Strategy]
    - [Test Framework Setup]
    - [Testing Strategy Examples]
    - [Code Coverage Requirements]
7. [Conclusion]
8. [Additional Components]
    - [Content Combiner]
    - [Panel Persistence]

---

# 1. Project Overview

## 1.1. Environment Setup Requirements

### 1.1.1. Development Tools Installation

To ensure a smooth development experience and avoid compatibility issues, please follow these detailed setup instructions:

1. **Node.js and npm/yarn**
    
    - **Node.js**: Install the latest Long Term Support (LTS) version from the [official Node.js website](https://nodejs.org/).
    - **npm**: Comes bundled with Node.js. Verify installation with `node -v` and `npm -v`.
    - **Yarn**: Install globally using npm: `npm install -g yarn`. Verify with `yarn -v`.
2. **TypeScript Compiler**
    
    - Install globally using npm: `npm install -g typescript`.
    - Verify installation with `tsc -v`.
3. **Visual Studio Code (VSCode)**
    
    - Download and install the latest stable version from the [VSCode website](https://code.visualstudio.com/).
    - Recommended extensions:
        - **ESLint**: For linting JavaScript/TypeScript code.
        - **Prettier**: For code formatting.
        - **TypeScript Hero**: For TypeScript development.
4. **Git**
    
    - Install the latest version for your operating system:
        
        - **Windows**: [Git for Windows](https://gitforwindows.org/)
        - **macOS**: Install via Homebrew: `brew install git`
        - **Linux**: Install via package manager, e.g., `sudo apt-get install git` for Debian-based systems.
    - Configure Git with your user information:
        
        ```bash
        git config --global user.name "Your Name"
        git config --global user.email "your.email@example.com"
        ```
        
5. **Disk Space**
    
    - Ensure at least 2GB of free disk space is available for the project setup and dependencies.

### 1.1.2. System Requirements

- **Operating System Compatibility Check**
    - Ensure compatibility with Windows 10+, macOS Catalina+, or modern Linux distributions.
- **Required Build Tools for Native Modules**
    - **Windows**: Install [Visual Studio Build Tools](https://visualstudio.microsoft.com/downloads/).
    - **macOS**: Install Xcode Command Line Tools: `xcode-select --install`.
    - **Linux**: Install build-essential: `sudo apt-get install build-essential`.

## 1.2. Project Creation Architecture

### 1.2.1. Base Project Structure

Organize the root directory with the following structure:

- `/src` - Source code
- `/config` - Configuration files
- `/assets` - Static resources
- `/build` - Build output
- `/types` - TypeScript type definitions

### 1.2.2. Configuration Files Setup

#### 1.2.2.1. TypeScript Configuration

Create `tsconfig.json` with the following settings:

- **Strict type checking**
- **Module resolution settings**
- **Source map configuration**
- **Target ECMAScript version**

#### 1.2.2.2. Electron Configuration

Set up Electron-specific configurations:

- **Main process settings**
- **Renderer process settings**
- **IPC communication setup**
- **Security policies**

### 1.2.3. Package Management

Categorize dependencies in `package.json`:

- **Production dependencies**
- **Development dependencies**
- **Optional dependencies**
- **Peer dependencies**

#### 1.2.3.1. Example of `package.json`

```json
{
  "name": "my-ai-writing-app",
  "version": "1.0.0",
  "description": "An AI-powered writing assistant application",
  "main": "src/main.ts",
  "scripts": {
    "start": "electron-forge start",
    "package": "electron-forge package",
    "make": "electron-forge make",
    "lint": "eslint . --ext .ts,.tsx",
    "test": "jest",
    "build": "tsc -p ."
  },
  "dependencies": {
    "electron": "^33.2.1",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "@reduxjs/toolkit": "^2.2.1",
    "react-redux": "^9.1.0",
    "@mui/material": "^5.15.12",
    "@mui/icons-material": "^5.15.12",
    "@emotion/react": "^11.11.4",
    "@emotion/styled": "^11.11.0",
    "openai": "^4.28.4",
    "electron-store": "^8.2.0",
    "keytar": "^7.9.0",
    "fs-extra": "^11.2.0",
    "zustand": "^4.4.1",
    "zustand-persist": "^1.0.0",
    "react-resizable": "^3.0.4",
    "react-markdown": "^8.0.5",
    "react-simple-code-editor": "^0.11.0",
    "react-error-boundary": "^3.1.4",
    "styled-components": "^6.0.5"
  },
  "devDependencies": {
    "@electron-forge/cli": "^7.0.0",
    "@electron-forge/maker-deb": "^7.0.0",
    "@electron-forge/maker-rpm": "^7.0.0",
    "@electron-forge/maker-squirrel": "^7.0.0",
    "@electron-forge/maker-zip": "^7.0.0",
    "typescript": "^5.2.2",
    "@types/react": "^18.2.18",
    "@types/react-dom": "^18.2.7",
    "@types/node": "^20.6.3",
    "vite": "^5.2.0",
    "@vitejs/plugin-react": "^4.0.0",
    "jest": "^29.7.0",
    "@testing-library/react": "^14.0.0",
    "@testing-library/jest-dom": "^6.0.0",
    "spectron": "^15.0.0",
    "nock": "^13.3.0",
    "sinon": "^15.2.0",
    "electron-devtools-installer": "^3.2.0",
    "eslint": "^8.0.0",
    "prettier": "^3.0.0",
    "ts-node": "^10.0.0",
    "electron-mocha": "^12.0.0",
    "chai": "^4.3.0",
    "typedoc": "^0.24.0",
    "jsdoc": "^4.0.0",
    "jsdoc-typescript": "^2.0.0",
    "http-server": "^14.1.1",
    "nodemon": "^3.0.1",
    "husky": "^8.0.0",
    "nyc": "^15.1.0",
    "coveralls": "^3.1.1"
  }
}
```

- **Dependencies**: Production dependencies required for the application to run.
- **DevDependencies**: Development dependencies required for building, testing, and packaging the application.

## 1.3. Logical Flow

### 1.3.1. Initialization Flow

#### 1.3.1.1. Environment Validation

- **Check Node.js version**
- **Verify TypeScript installation**
- **Validate build tools**

#### 1.3.1.2. Project Scaffolding

- **Create directory structure**
- **Initialize Git repository**
- **Generate `package.json`**
- **Configure TypeScript**

#### 1.3.1.3. Development Environment Setup

- **Install development dependencies**
- **Configure build scripts**
- **Set up development tools**

### 1.3.2. Build Process Flow

#### 1.3.2.1. Source Compilation

- **TypeScript to JavaScript**
- **Asset processing**
- **Bundle optimization**

#### 1.3.2.2. Development Workflow

- **Hot reload setup**
- **Debug configuration**
- **Source map generation**

### 1.3.3. Testing Strategy

#### 1.3.3.1. Unit Testing Setup

- **Test framework configuration**
- **Mock system setup**
- **Test runner integration**

#### 1.3.3.2. Integration Testing

- **Cross-platform testing**
- **IPC communication testing**
- **Window management testing**

## 1.4. Development Workflow

### 1.4.1. Version Control Strategy

- **Git workflow**
- **Branch management**
- **Commit conventions**
- **Release tagging**

### 1.4.2. Documentation Requirements

#### 1.4.2.1. Code Documentation

- **JSDoc comments**
- **API documentation**
- **Usage examples**

#### 1.4.2.2. Project Documentation

- **Setup instructions**
- **Development guidelines**
- **Troubleshooting guide**

## 1.5. UI Library and Component Ecosystem

The user interface will be built using the following libraries:

- **React (v18.2.0):** The core UI library, chosen for its component-based architecture, performance, and strong TypeScript support.
- **Material-UI (MUI) (v5.15.12):** A comprehensive component library providing pre-built UI elements and styling.
- **react-resizable (v3.0.4):** For creating resizable panels.
- **react-markdown (v8.0.5):** For rendering Markdown content in the AI Feedback panel.
- **react-simple-code-editor (v0.11.0):** For the text editor component.
- **react-error-boundary (v3.1.4):** For implementing error boundaries in React components.
- **styled-components (v6.0.5):** For styling and theming the application.
- **Zustand (v4.4.1):** For state management within React components.
- **zustand-persist (v1.0.0):** Middleware for persisting Zustand state.

These libraries were chosen for their maturity, community support, and good integration with TypeScript and React.

---

# 2. Architecture Analysis

## 2.1. Configuration Architecture Analysis

### 2.1.1. Configuration Directory Structure

Organize the `config/` directory into distinct configuration files based on functionality.

### 2.1.2. Core Configuration Files Needed

1. **Application Configuration** (`appConfig.ts`)
    
    - Basic app settings like window dimensions, default paths
    - Environment-specific settings (dev/prod)
    - App-wide constants
2. **Panel Configuration** (`panelConfig.ts`)
    
    - Layout settings for the three main panels
    - Default dimensions and constraints
    - Panel-specific behavior settings
3. **File System Configuration** (`fileSystemConfig.ts`)
    
    - File handling settings
    - Permitted file types and extensions
    - Default save/load paths
    - File watching configurations
4. **OpenAI Configuration** (`openAIConfig.ts`)
    
    - API configuration settings
    - Model parameters
    - Rate limiting settings
    - Response formatting preferences
5. **UI Configuration** (`uiConfig.ts`)
    
    - Theme settings
    - Font configurations
    - UI element sizing
    - Animation settings

## 2.2. Configuration Management System

### 2.2.1. Configuration Loading Flow

- **Initial load during app startup**
- **Environment detection** (dev/prod)
- **User preferences merge**
- **Configuration validation**
- **Configuration distribution to modules**

### 2.2.2. Configuration Updates Handling

- **Runtime configuration changes**
- **Persistence of user preferences**
- **Configuration change event propagation**
- **Validation of changes**
- **Rollback mechanism for invalid changes**

## 2.3. Type System

### 2.3.1. Configuration Interfaces

- **Base configuration interface**
- **Module-specific configuration interfaces**
- **Validation schemas**
- **Type guards for runtime checking**

### 2.3.2. Type Safety

- **Strict typing for all configuration values**
- **Runtime type checking**
- **Default value handling**
- **Type coercion rules**

## 2.4. Configuration Access Pattern

### 2.4.1. Configuration Service

- **Singleton pattern for global access**
- **Cached configuration values**
- **Change notification system**
- **Configuration inheritance hierarchy**

### 2.4.2. Access Control

- **Read-only vs. modifiable settings**
- **Environment-specific restrictions**
- **Module-level access control**
- **Configuration change audit trail**

## 2.5. Integration Points

### 2.5.1. Module Integration

- **Configuration injection patterns**
- **Module initialization sequence**
- **Configuration dependency resolution**
- **Error handling for missing configurations**

### 2.5.2. Cross-Module Configuration

- **Shared configuration values**
- **Configuration value inheritance**
- **Configuration conflict resolution**
- **Default fallback values**

## 2.6. Security Considerations

### 2.6.1. Sensitive Data

- **Encryption of sensitive values**
- **Secure storage of credentials**
- **Environment variable integration**
- **Secret management**

### 2.6.2. Access Control

- **Configuration read/write permissions**
- **Configuration value validation**
- **Audit logging of changes**
- **Security boundary enforcement**

## 2.7. Error Handling

### 2.7.1. Configuration Errors

- **Missing configuration detection**
- **Invalid value handling**
- **Type mismatch resolution**
- **Required vs. optional settings**

### 2.7.2. Error Recovery

- **Default value fallbacks**
- **Configuration reset capability**
- **Error notification system**
- **Recovery logging**

## 2.8. Testing Strategy

### 2.8.1. Configuration Testing

- **Configuration loading tests**
- **Validation tests**
- **Type safety tests**
- **Integration tests**

### 2.8.2. Error Condition Testing

- **Missing configuration tests**
- **Invalid value tests**
- **Permission boundary tests**
- **Recovery mechanism tests**

---

# 3. Application Design and Implementation

## 3.1. Front-end/UI Design

### 3.1.1. Application Frame Implementation Plan

#### 3.1.1.1. Main Window Creation Requirements

##### 3.1.1.1.1. Core Dependencies

- **Electron's `BrowserWindow` module**
- **Node.js `path` module for file path handling**
- **Configuration files for window settings**

##### 3.1.1.1.2. Window Configuration Considerations

###### Window Dimensions

- Define default width and height
- Handle minimum window size constraints
- Consider screen size adaptability

###### Window Properties

- Title bar configuration
- Frame styling (native or custom)
- Window state management (maximized, minimized, etc.)
- Focus and blur event handling

###### Security Settings

- Context isolation configuration
- Node integration settings
- Sandbox configuration
- Content Security Policy implementation

#### 3.1.1.2. Panel Layout Structure

##### 3.1.1.2.1. Layout Requirements

###### Three-Panel Design

- **Left Panel**: Writing Prompts & AI Assistant
- **Middle Panel**: Text Editor
- **Right Panel**: AI Feedback, Select Criteria, Settings

###### Panel Behavior

- Resizable panels with minimum/maximum constraints
- Collapsible panels
- Panel state persistence
- Independent scroll containers

###### Panel Communication

- Inter-panel messaging
- Event system for panel communication
- State management between panels
- Data flow architecture

### 3.1.2. Overall UI Architecture

#### 3.1.2.1. Core UI Framework Selection

We will use **React** with **TypeScript** as the UI framework throughout the application. This choice ensures consistency and leverages React's strengths for building complex user interfaces.

**Reasons for Choosing React:**

- **Component-Based Architecture**: Aligns well with the panel structure of the application.
- **TypeScript Integration**: Offers robust type safety and better tooling support.
- **State Management**: Works seamlessly with libraries like Zustand for state management.
- **Large Ecosystem**: Access to a wide range of UI components and community support.
- **Developer Tools**: Enhanced debugging and development experience with React DevTools.

#### 3.1.2.2. Layout Structure

##### Main Window Container

- Use React components to define the main layout.
- Implement a flexible content area using CSS Grid or Flexbox.
- Ensure minimum window dimensions to prevent layout breaking.

##### Panel Management System

- Each panel is a React component.
- Implement resizable panels using React libraries like `react-resizable`.
- Maintain state for panel sizes and active tabs.

#### 3.1.2.3. Panel-Specific Requirements

##### Left Panel Implementation

- **Tab System**: Implement using React components for tabs.
    
    - **Writing Prompts Tab**:
        
        - **Folder Selection Interface**: Use a React component that interacts with Electron's `dialog` module.
        - **Category Selection**: Use a dropdown component to display subfolders.
        - **Prompt Management**: Implement buttons like **Refresh Categories**, **Get New Prompt**, and **Use This Prompt** using React event handlers.
        - **Prompt Display**: Display prompts using React components with state management.
    - **AI Assistant Tab**:
        
        - **Chat Interface**: Use React components styled to resemble iOS Messenger.
        - **Conversation History**: Manage state using React state or Zustand.
        - **Controls**: Implement **Clear Chat** and message input using React components.

##### Middle Panel Implementation

- **Text Editor Requirements**:
    
    - Use a React component for the text editor, leveraging libraries like `react-simple-code-editor` or `@monaco-editor/react`.
    - Implement word count, auto-save, undo/redo, and copy/paste using React state and effects.
    - **File Saving**: Use Electron APIs via the preload script for saving files.
- **Action Buttons**:
    
    - Implement **Submit for Feedback** and **Clear** using React components with appropriate event handlers.

##### Right Panel Implementation

- **Tab Management**: Implement using React components.
    
    - **AI Feedback Tab**: Use a markdown viewer component like `react-markdown`.
    - **Select Criteria Tab**: Implement folder and file selection interfaces using React components.
    - **Settings Tab**: Use forms and inputs to manage application settings.

#### 3.1.2.4. State Management

We will use **Zustand** for state management in React components. To integrate persistence with Zustand, we will use the `zustand-persist` middleware.

**Persistence Strategy:**

- **Middleware Integration**: Apply `zustand-persist` to store slices that need persistence.
- **Storage Mechanism**: Use Electron's `localStorage` or `electron-store` for persistent storage.
- **State Synchronization**: Ensure that persisted state is synchronized across sessions.

**Implementation Example:**

```typescript
import create from 'zustand';
import { persist } from 'zustand/middleware';

interface AppState {
  window: WindowState;
  panels: PanelsState;
  editor: EditorState;
  aiAssistant: AIAssistantState;
  userPreferences: UserPreferencesState;
  // Actions
  setWindowState: (state: WindowState) => void;
  setPanelState: (panel: string, state: any) => void;
  // Other actions...
}

export const useStore = create<AppState>()(
  persist(
    (set, get) => ({
      // Initial state
      window: { /* default window state */ },
      panels: { /* default panels state */ },
      editor: { /* default editor state */ },
      aiAssistant: { /* default AI assistant state */ },
      userPreferences: { /* default user preferences */ },
      // Actions
      setWindowState: (state) => set({ window: { ...get().window, ...state } }),
      setPanelState: (panel, state) =>
        set((prevState) => ({
          panels: {
            ...prevState.panels,
            [panel]: { ...prevState.panels[panel], ...state },
          },
        })),
      // Other actions...
    }),
    {
      name: 'app-state',
      getStorage: () => window.localStorage, // or use electron-store
    }
  )
);
```

This approach ensures a centralized and consistent state management throughout the application.

#### 3.1.2.5. Error Boundaries in React Components

To gracefully handle errors in the renderer process, we will implement **Error Boundaries** in our React components.

**Implementation Details:**

- Create a high-level Error Boundary component using React's `componentDidCatch` lifecycle method or the `ErrorBoundary` component from `react-error-boundary`.
- Wrap key components or panels with the Error Boundary to catch rendering errors.
- Display a fallback UI when an error occurs, providing options to retry or report the issue.
- Log errors to a monitoring service or console for debugging.

**Example:**

```jsx
import React from 'react';
import { ErrorBoundary } from 'react-error-boundary';

function FallbackComponent({ error, resetErrorBoundary }) {
  return (
    <div role="alert">
      <p>An error occurred: {error.message}</p>
      <button onClick={resetErrorBoundary}>Try Again</button>
    </div>
  );
}

function App() {
  return (
    <ErrorBoundary FallbackComponent={FallbackComponent}>
      {/* Rest of the app components */}
    </ErrorBoundary>
  );
}
```

#### 3.1.2.6. Event Handling

##### User Interactions

- Use React's event system to handle clicks, inputs, and other user actions.
- Implement debounce or throttle where necessary to optimize performance.

##### System Events

- Use React hooks like `useEffect` to handle component lifecycle events.
- Integrate with Electron's IPC system via the preload script for system-level events.

#### 3.1.2.7. Cross-Panel Communication

##### Event Bus

- Use Zustand's global store to manage state across panels.
- Implement actions and selectors for state updates.
- Ensure immutability and state predictability.

#### 3.1.2.8. Styling Requirements

##### Theme System

- Use CSS-in-JS libraries like `styled-components` or `@emotion/styled` for dynamic theming.
- Implement dark and light modes.

##### Responsive Design

- Use media queries and responsive units to adjust layouts.
- Ensure accessibility across different screen sizes.

#### 3.1.2.9. Accessibility Requirements

##### Implementation Needs

- Use semantic HTML elements in React components.
- Ensure all interactive elements are keyboard navigable.
- Add ARIA attributes where necessary.

## 3.2. Back-end Features

### 3.2.1. OS Manipulation Implementation

#### 3.2.1.1. File System Configuration

- Define allowed file types and default paths.
- Use Electron's `fs` and `dialog` modules for file operations.

#### 3.2.1.2. File System Service

- Implement methods for file selection, reading, and writing.
- Use async/await for non-blocking operations.
- Handle conversation history using `electron-store`.

### 3.2.2. File Manipulation Implementation

#### 3.2.2.1. Content Combiner Configuration

- Define rules for combining content in `contentCombinerConfig.ts`.

#### 3.2.2.2. Content Combiner Service

- Implement validation and formatting methods.
- Ensure combined content adheres to OpenAI's token limits.

### 3.2.3. OpenAI Integration Implementation

#### 3.2.3.1. OpenAI Configuration

- Manage API settings and behavior in `openAIConfig.ts`.

#### 3.2.3.2. API Key Management

- Use `keytar` for secure storage of API keys.
- Provide a settings interface in the UI for key input.

#### 3.2.3.3. OpenAI API Calls

- Use the OpenAI Node.js library.
- Handle conversation context for the AI Assistant.

## 3.3. Implementation Flow

### 3.3.1. Dependencies and Flow

- Ensure all modules interact seamlessly.
- Maintain a clear data flow from user actions to API responses.

### 3.3.2. Initialization Flow

- Load configurations.
- Initialize services.
- Render the main application UI.

### 3.3.3. Error Handling Flow

- Validate inputs.
- Handle exceptions.
- Provide user feedback.

### 3.3.4. Data Flow

- User actions trigger state changes.
- State changes result in UI updates or API calls.
- API responses update the state and UI.

## 3.4. Cross-Platform Compatibility

- Ensure UI elements behave consistently across platforms.
- Use platform-specific features where appropriate.

## 3.5. Performance Considerations

- Optimize rendering with React's `memo` and `useCallback`.
- Manage state efficiently with Zustand.

## 3.6. Error Handling Requirements

- Implement try/catch blocks around asynchronous operations.
- Use error boundaries in React components.

## 3.7. Testing Requirements

- Write unit tests for all services and components.
- Use mocks to simulate API responses and file system operations.

## 3.8. Documentation Needs

- Provide comprehensive code documentation.
- Update user guides to reflect new features.

## 3.9. Accessibility Requirements

- Ensure the application meets WCAG 2.1 AA standards.

## 3.10. Conclusion

This architecture ensures a robust, maintainable, and secure application while providing a solid foundation for all the application's features. The implementation should follow this structure to maintain consistency and reliability across the application.

---

# 4. Implementation Details

## 4.1. Set Up Electron Main Process

**Objective:** Initialize the main process to manage the app lifecycle.

### 4.1.1. Create a Main Script

- Create `main.ts` as the entry point of the application.

### 4.1.2. Initialize the App

- Use Electron's `app` module to handle the application lifecycle.

### 4.1.3. Create a Browser Window

- Use the `BrowserWindow` class to create the main application window.

### 4.1.4. Load HTML Content

- Use `loadFile` or `loadURL` to load the initial HTML file into the window.

### 4.1.5. Handle App Events

- Ensure the app quits when all windows are closed.

## 4.2. Create Preload Script

**Objective:** Expose necessary APIs to the renderer process.

### 4.2.1. Purpose of Preload Script

- Runs in the renderer process but has access to Node.js APIs.

### 4.2.2. Security Considerations

- Enable `contextIsolation` in the `webPreferences` of `BrowserWindow`.
- Use `contextBridge` to expose only the necessary APIs.

### 4.2.3. Implement Preload Script

- Create `preload.ts`.

## 4.3. Set Up IPC Communication

**Objective:** Enable communication between the main and renderer processes.

### 4.3.1. Use IPC Modules

- Utilize `ipcMain` in the main process.
- Utilize `ipcRenderer` in the renderer process via the preload script.

### 4.3.2. Define Communication Channels

- Establish specific channels for different message types.

### 4.3.3. Handle IPC Events

- Use `ipcMain.handle` and `ipcRenderer.invoke` for asynchronous communication.

### 4.3.4. Security Best Practices

- Validate and sanitize all data received through IPC.

---

# 5. Best Practices

## 5.1. Component Registration System

- Create a centralized registry for React components.
- Use dynamic imports for code splitting.

## 5.2. Type-First Development

- Strictly type all components and services.
- Use TypeScript interfaces and types.

## 5.3. Documentation System

- Use JSDoc and TypeDoc for code documentation.
- Maintain a `docs/` directory for architecture and user guides.

## 5.4. Error Handling Strategy

- Categorize errors and handle them appropriately.
- Use React Error Boundaries for UI errors.

## 5.5. Performance Considerations

- Optimize React rendering.
- Use memoization where applicable.

## 5.6. Security Measures

- Sanitize all inputs.
- Use secure storage for sensitive data.

## 5.7. Testing Strategy Overview

- Use Jest for unit testing.
- Use `jest.mock()` to mock modules.
- Use `nock` to intercept HTTP requests.

## 5.8. Error Monitoring and Reporting

- Implement logging for critical errors.
- Consider integrating a service like Sentry.

---

# 6. Testing Strategy

## 6.1. Test Framework Setup

### 6.1.1. Core Testing Stack

- **Jest**: For unit and integration tests.
- **React Testing Library**: For testing React components.
- **Spectron**: For end-to-end Electron testing.
- **nock**: For mocking HTTP requests.
- **Sinon**: For spies, stubs, and mocks.

## 6.2. Testing Strategy Examples

### 6.2.1. Mock Implementations

#### Mocking OpenAI API with `nock`

```javascript
// test/mocks/openai.ts
import nock from 'nock';

export const mockOpenAI = () => {
  nock('https://api.openai.com')
    .persist()
    .post('/v1/engines/davinci/completions')
    .reply(200, {
      id: 'mocked-id',
      choices: [{ text: 'Mocked response from OpenAI' }],
    });
};
```

#### Using `jest.mock()` for File System

```javascript
// test/mocks/fs.ts
jest.mock('fs', () => {
  return {
    promises: {
      readFile: jest.fn().mockResolvedValue('Mocked file content'),
      writeFile: jest.fn().mockResolvedValue(undefined),
    },
  };
});
```

### 6.2.2. Integration Test Example - OpenAI API

```typescript
// test/integration/openai.spec.ts
import { OpenAIService } from '../../src/services/openai';
import { mockOpenAI } from '../mocks/openai';

describe('OpenAI Integration', () => {
  beforeAll(() => {
    mockOpenAI();
  });

  it('should return mocked response', async () => {
    const openaiService = new OpenAIService();
    const response = await openaiService.getCompletion('Test prompt');
    expect(response).toBe('Mocked response from OpenAI');
  });
});
```

### 6.2.3. Unit Test Example - File System Operations

```typescript
// test/unit/file-system.spec.ts
import { FileSystemService } from '../../src/services/file-system';

jest.mock('fs');

describe('FileSystemService', () => {
  it('should read file content', async () => {
    const fsService = new FileSystemService();
    const content = await fsService.readFile('/path/to/file.txt');
    expect(content).toBe('Mocked file content');
  });
});
```

## 6.3. Code Coverage Requirements

### 6.3.1. Coverage Thresholds

- **Statements**: 85%
- **Branches**: 80%
- **Functions**: 85%
- **Lines**: 85%

### 6.3.2. Coverage Reporting

```json
// package.json
{
  "scripts": {
    "test:coverage": "jest --coverage",
    "coverage:report": "jest --coverage --coverageReporters=text-lcov | coveralls"
  }
}
```

### 6.3.3. CI Integration

```yaml
# .github/workflows/test.yml
name: Test and Coverage

on: [push, pull_request]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - name: Install dependencies
        run: yarn install --frozen-lockfile
      - name: Run Tests with Coverage
        run: yarn test:coverage
      - name: Upload Coverage to Coveralls
        uses: coverallsapp/github-action@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

---

# 7. Conclusion

This comprehensive strategy ensures:

- **Clear separation of concerns**
- **Robust error handling**
- **Secure communication**
- **Maintainable codebase**
- **Testable components**
- **Performance optimization**
- **User-friendly experience**

By following these structured guidelines and implementation plans, you can develop a robust, secure, and cross-platform AI-assisted writing application using Electron, React, and TypeScript. The focus on best practices, security considerations, and proper documentation will ensure maintainability and scalability as the project evolves.

---

# 8. Additional Components

## 8.1. Content Combiner

### `contentCombinerConfig.ts`

```typescript
interface SectionConfig {
  prefix: string;
  suffix: string;
  required: boolean;
  maxLength?: number;
  trim?: boolean;
}

interface ContentCombinerConfig {
  sections: {
    prompt: SectionConfig;
    submission: SectionConfig;
    criteria: SectionConfig;
  };
  separator: string;
  maxTotalLength: number;
  preserveWhitespace: boolean;
  errorMessages: {
    maxLengthExceeded: string;
    missingRequired: string;
    invalidContent: string;
  };
}

export const CONTENT_COMBINER_CONFIG: ContentCombinerConfig = {
  sections: {
    prompt: {
      prefix: "===Writing Prompt===\n",
      suffix: "\n=====\n",
      required: false,
      maxLength: 2000,
      trim: true,
    },
    submission: {
      prefix: "===User Submission===\n",
      suffix: "\n=====\n",
      required: true,
      maxLength: 8000,
      trim: false,
    },
    criteria: {
      prefix: "===Evaluation Criteria===\n",
      suffix: "\n=====\n",
      required: true,
      maxLength: 2000,
      trim: true,
    },
  },
  separator: "\n",
  maxTotalLength: 12000,
  preserveWhitespace: false,
  errorMessages: {
    maxLengthExceeded: "Content exceeds maximum allowed length",
    missingRequired: "Required section is missing",
    invalidContent: "Invalid content provided",
  },
};
```

### `ContentCombinerService.ts`

```typescript
import { CONTENT_COMBINER_CONFIG } from '../config/contentCombinerConfig';

export class ContentValidationError extends Error {
  constructor(message: string, public section?: string) {
    super(message);
    this.name = 'ContentValidationError';
  }
}

export class ContentCombinerService {
  private validateSection(content: string, section: keyof typeof CONTENT_COMBINER_CONFIG.sections): void {
    const config = CONTENT_COMBINER_CONFIG.sections[section];

    if (!content && config.required) {
      throw new ContentValidationError(
        CONTENT_COMBINER_CONFIG.errorMessages.missingRequired,
        section
      );
    }

    if (config.maxLength && content.length > config.maxLength) {
      throw new ContentValidationError(
        CONTENT_COMBINER_CONFIG.errorMessages.maxLengthExceeded,
        section
      );
    }
  }

  private formatSection(content: string, section: keyof typeof CONTENT_COMBINER_CONFIG.sections): string {
    const config = CONTENT_COMBINER_CONFIG.sections[section];
    let processedContent = content || '';

    if (config.trim) {
      processedContent = processedContent.trim();
    }

    if (!processedContent && !config.required) {
      return ''; // Skip optional empty sections
    }

    return `${config.prefix}${processedContent}${config.suffix}`;
  }

  public combineContent(prompt: string | null, submission: string, criteria: string): string {
    // Validate all sections
    if (prompt !== null) {
      this.validateSection(prompt, 'prompt');
    }
    this.validateSection(submission, 'submission');
    this.validateSection(criteria, 'criteria');

    // Format each section
    const formattedPrompt = this.formatSection(prompt || '', 'prompt');
    const formattedSubmission = this.formatSection(submission, 'submission');
    const formattedCriteria = this.formatSection(criteria, 'criteria');

    // Combine sections
    const combinedContent = [
      formattedPrompt,
      formattedSubmission,
      formattedCriteria,
    ]
      .filter((section) => section) // Remove empty sections
      .join(CONTENT_COMBINER_CONFIG.separator);

    // Validate total length
    if (combinedContent.length > CONTENT_COMBINER_CONFIG.maxTotalLength) {
      throw new ContentValidationError(
        CONTENT_COMBINER_CONFIG.errorMessages.maxLengthExceeded
      );
    }

    return combinedContent;
  }
}
```

## 8.2. Panel Persistence

### 8.2.1. Panel State Persistence

#### Implementation Details

Panel state persistence is managed using the **`useStore`** hook and **`zustand-persist`** middleware, as described in the State Management section ([3.1.2.4. State Management]). This ensures a centralized and consistent approach to state management throughout the application.

#### Data to be Persisted

- **Window State**
- **Panel Layout**
- **Tab States**
- **Editor State**
- **AI Assistant State**
- **User Preferences**

#### State Management Service

Refer to the `useStore` implementation in section [3.1.2.4. State Management] for the detailed code example and explanation on how the application state is managed and persisted using Zustand and `zustand-persist`.

---
