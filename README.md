# Elevate Task 3

## Project Overview
This project demonstrates a DevOps workflow using Git best practices. It includes a basic Express.js application and follows a structured branching strategy.

## Steps to Complete Task 3

### 1. Initialize Git Repository
- A Git repository was initialized using the command:
  ```bash
  git init
  ```
- A `.gitignore` file was created to exclude unnecessary files such as `node_modules` and `.env`.

### 2. Push to GitHub
- A new GitHub repository was created and linked to the local repository using the GitHub CLI:
  ```bash
  gh repo create elevate-task3 --public --source=. --remote=origin --push
  ```

### 3. Create Branches
- The following branches were created to follow Git best practices:
  - `main`: The production-ready branch.
  - `dev`: The development branch where features are integrated.
  - `feature`: Branches for individual features.
- Commands used:
  ```bash
  git checkout -b dev
  git push -u origin dev
  git checkout -b feature
  git push -u origin feature
  ```

### 4. Use Pull Requests
- Pull requests are used to merge changes from `feature` to `dev` and from `dev` to `main`.

### 5. Add Git Tags
- Git tags are used to mark important milestones or releases. Example:
  ```bash
  git tag -a v1.0 -m "Initial release"
  git push origin v1.0
  ```

## Setup Instructions
1. Clone the repository:
   ```bash
   git clone <repository-url>
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the application:
   ```bash
   npm start
   ```

## Usage
Visit `http://localhost:3000` to see the application running.

## Contributing
1. Create a new feature branch:
   ```bash
   git checkout -b feature/<feature-name>
   ```
2. Commit your changes and push to GitHub.
3. Open a pull request to merge into `dev`.
