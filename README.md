# Country Explorer Project

This project allows users to explore countries, view random countries with their next holiday, and see detailed holiday information for a selected country in a chosen year. The data is fetched from the open-source Nager.Date API.

## Project Overview

- **Home Page**:
  - Allows users to search for countries.
  - Displays 3 random countries with their next holiday.
  - Navigation to the selected country's page.

- **Country Page**:
  - Shows holidays for the selected country in the current year.
  - Allows users to view holidays for a selected year (2020–2030).

## Tech Stack

- **Frontend**: 
  - **Nuxt.js** (for building the application)
  - **TailwindCSS** (for styling)
  - **TypeScript** (for type safety)
  - **Composition API** (for managing application state)

- **API**: 
  - **Nager.Date API** ([API Documentation](https://date.nager.at/swagger/index.html))

## Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd <project-directory>

## Setup

Make sure to install the dependencies:

```bash
# npm
npm install

## Development Server

Start the development server on `http://localhost:3000`:

```bash
# npm
npm run dev
