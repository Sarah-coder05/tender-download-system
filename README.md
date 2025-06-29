# Tender Download System
This is a modern, responsive web application built with Vue 3 and Tailwind CSS, designed to simulate a system for browsing, searching, and downloading tender documents. It features a clean user interface, dynamic content filtering, pagination, detailed tender views, and a static authentication system.

Table of Contents
# Features

# Technologies Used

# Setup Instructions

# Usage

# Project Structure

# Author

## Features
Tender Listing: Displays a paginated list of tender documents.

Search Functionality: Allows users to search tenders by title and description with an integrated search icon in the header.

Document Download: Enables users to download tender-related documents (simulated with .txt files).

Pagination: Organizes tenders into multiple pages with "Previous" and "Next" navigation, displaying only the current page number.

Tender Detail Modal: Clicking on a tender card opens a modal displaying comprehensive details of the selected tender.

Static User Authentication: A simplified login system with hardcoded credentials for demonstration purposes (no backend required).

Responsive Design: Adapts seamlessly to various screen sizes (desktop, tablet, mobile), including a sticky header and flexible content layout.

Robust Error Handling: Displays user-friendly messages and a retry option if tender data fails to load.

Loading State: Provides a visual loading indicator during data fetching with an artificial delay for demonstration.

# Technologies Used
Vue.js 3 (Composition API): Reactive framework for building user interfaces.

TypeScript: For type-safe JavaScript development.

Tailwind CSS: A utility-first CSS framework for rapid and responsive UI development.

fetch API: For simulating data fetching from a local JSON file.

# Setup Instructions
To get this project up and running on your local machine:

Clone the repository (if applicable):
If this project were in a Git repository, you would clone it first:

git clone <https://github.com/Sarah-coder05/tender-download-system>
cd tender-download-system


Install Dependencies:
Use npm to install the necessary project dependencies:

npm install

# Prepare Dummy Data:

Ensure you have a public folder in your project root.

Create tenders.json inside the public folder with the tender data.

Create dummy .txt files (e.g., tender_doc_1.txt, tender_doc_2.txt, etc.) inside the public folder. The content for these files should be placed inside them.

# Run the Development Server:
Start the local development server:

npm run serve

The application will typically be available at http://localhost:8080/ (or another port if 8080 is in use).

# Usage
Access the Application: Open your web browser and navigate to the address provided by your development server (e.g., http://localhost:8080/).

# Login:

You will first be presented with a login form.

Use the following static credentials:

Email: user@ufedo.com

Password: Abcd123

Click the "Login" button to access the tender list.

# Browse Tenders:

Once logged in, you will see a list of available tenders.

Use the "Previous" and "Next" buttons at the bottom to navigate through pages. The current page number will be displayed.

# Search Tenders:

Use the search input field in the header to filter tenders by their title or description. The pagination will reset to the first page for filtered results.

# View Tender Details:

Click anywhere on a tender card (excluding the "Download Tender" button) to open a modal window with more detailed information about that specific tender.

Click the "Close" button or the 'X' icon to dismiss the modal.

# Download Tender Documents:

Click the "Download Tender" button on any tender card to download the associated document. The browser will handle the download process.

# Logout:

Click the "Logout" button in the header to return to the login screen.

# Project Structure
tender-download-system/
├── public/                 # Static assets (index.html, tenders.json, tender_doc_X.txt)
│   ├── index.html          # Main HTML file
│   ├── tenders.json        # Mock data for tenders
│   └── tender_doc_1.txt    # Example tender document (and others up to 9)
├── src/
│   ├── assets/             # Static assets (e.g., images if any)
│   ├── components/
│   │   ├── AuthForm.vue      # Static login form
│   │   ├── SearchBar.vue     # Search input component
│   │   ├── TenderDetailModal.vue # Modal for tender details
│   │   └── TenderList.vue    # Displays the list of tenders
│   ├── App.vue             # Main application component
│   ├── main.ts             # Vue app initialization
│   └── shims-vue.d.ts      # TypeScript declaration file for .vue imports
├── .eslintrc.js            # ESLint configuration
├── package.json            # Project dependencies and scripts
├── postcss.config.js       # PostCSS configuration for Tailwind CSS
├── README.md               # This file
├── tsconfig.json           # TypeScript configuration
└── tailwind.config.js      # Tailwind CSS configuration


Sarah Moses
https://github.com/Sarah-coder05tender-download-system