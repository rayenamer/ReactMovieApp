
# Movie App - React

## Overview

This project is a simple movie app built with React that allows users to search for movies, view popular movies, and manage their favorite movies.
The app uses **The Movie Database (TMDb) API** for fetching movie data and **React Context** for state management.

## Features

- Search for movies by name.
- View popular movies.
- Add movies to favorites.
- Remove movies from favorites.
- View a list of favorite movies.

## Project Structure

```
ReactMovieApp
├── public
│   └── vite.svg
├── src
│   ├── assets
│   │   └── react.svg
│   ├── components
│   │   ├── MovieCard.jsx
│   │   └── NavBar.jsx
│   ├── contexts
│   │   └── MovieContext.jsx
│   ├── css
│   │   ├── App.css
│   │   ├── Favorites.css
│   │   ├── Home.css
│   │   ├── MovieCard.css
│   │   ├── NavBar.css
│   │   └── index.css
│   ├── pages
│   │   ├── Favorites.jsx
│   │   └── Home.jsx
│   ├── services
│   │   └── api.js
│   ├── App.jsx
│   ├── main.jsx
│   └── .gitignore
├── README.md
├── eslint.config.js
├── index.html
├── package-lock.json
├── package.json
└── vite.config.js
```

## Setup and Installation

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/movie-app.git
```

### 2. Install dependencies

```bash
cd movie-app
npm install
```

### 3. Run the development server

```bash
npm run dev
```

Visit `http://localhost:3000` to view the app.

---

## Components Breakdown

### 1. **MovieCard.jsx**

This component represents a single movie card displayed in the app.

- **Props**:
  - `movie`: An object representing the movie with details like `id`, `title`, `release_date`, and `poster_path`.

- **State**:
  - The component uses `useMovieContext` to access context functions like `addToFavorites`, `removeFromFavorites`, and `isFavorite`.

- **Main Logic**:
  - If the movie is a favorite, it shows an active heart button. Clicking the heart toggles the favorite status by either adding or removing the movie from the `favorites` list.

### 2. **NavBar.jsx**

The navigation bar contains links to the home page and the favorites page.

- **Links**:
  - **Home**: The main page displaying popular movies and the search functionality.
  - **Favorites**: A page displaying the list of user's favorite movies.

### 3. **MovieContext.jsx**

This file contains the context used for managing the state of favorite movies across the app.

- **State**:
  - `favorites`: An array of movies marked as favorites by the user.

- **Functions**:
  - `addToFavorites`: Adds a movie to the `favorites` array.
  - `removeFromFavorites`: Removes a movie from the `favorites` array.
  - `isFavorite`: Checks if a movie is already in the favorites list.

- **LocalStorage**:
  - The `favorites` list is persisted in `localStorage`, so users' favorite movies are saved even if the page is reloaded.

### 4. **Favorites.jsx**

This page shows the list of movies marked as favorites.

- **State**:
  - It accesses the `favorites` array from the `MovieContext`.

- **Main Logic**:
  - If there are no favorite movies, a message is displayed. Otherwise, it maps over the `favorites` array and renders a `MovieCard` for each movie.

### 5. **Home.jsx**

This page is where users can see popular movies or search for movies.

- **State**:
  - `searchQuery`: Holds the current search query.
  - `movies`: Stores the list of movies either fetched from popular movies or search results.
  - `error`: Stores any error messages when fetching movies.
  - `loading`: Indicates if data is currently being fetched.

- **Main Logic**:
  - Upon loading, the page fetches a list of popular movies using `getPopularMovies`.
  - When a user submits a search, it calls `searchMovies` to fetch movies matching the query.
  - Displays loading, error messages, and movie cards accordingly.

### 6. **api.js**

This file contains functions for interacting with The Movie Database (TMDb) API.

- **`getPopularMovies`**: Fetches a list of popular movies.
- **`searchMovies`**: Searches for movies based on a query string.

### 7. **App.jsx**

- **Routing**:
  - The app uses `React Router` for navigating between the `Home` and `Favorites` pages.

- **Context**:
  - The `MovieProvider` is wrapped around the entire app to provide access to movie-related context in all components.

### 8. **main.jsx**

This is the entry point for the React app. It initializes the app with strict mode and enables routing with `BrowserRouter`.


## Variables Used in the Application

### 1. **React State Variables**

State in React allows you to store data that can change over time and trigger a re-render of your component when it does. In this application, state variables are used to track user input, loading states, movie data, and errors.

#### **`useState` Hook**

The `useState` hook is used to create state variables inside functional components. It returns an array containing the current state and a function to update it.

```jsx
const [searchQuery, setSearchQuery] = useState("");  // Initializing an empty search query.
```

Here’s a breakdown of how `useState` is used across the app:

- **`searchQuery`**: Stores the user's search query input. Initially, it is an empty string.
  - Example: When a user types in the search box, `searchQuery` is updated with the new value.
  
```jsx
<input
  type="text"
  placeholder="Search for movies..."
  className="search_input"
  value={searchQuery}  // Search query state is bound to the input field
  onChange={(e) => setSearchQuery(e.target.value)}  // Updates the search query on change
/>
```

- **`movies`**: Holds the list of movies fetched from either the popular movie API or the search results.
  - Example: After fetching popular movies, this state is updated with the list of popular movies. The state is also updated with search results when a user submits the search form.
  
```jsx
const [movies, setMovies] = useState([]);  // Initially an empty array
```

- **`loading`**: Tracks the loading state to display a loading spinner or message when data is being fetched.
  - Example: When the app starts fetching movies, `loading` is set to `true`, and while waiting for the response, the user sees a loading indicator.

```jsx
const [loading, setLoading] = useState(true);  // Initially set to true to show loading state
```

- **`error`**: Stores any error messages that may arise during the fetching process.
  - Example: If the API request fails, the `error` state is updated, and an error message is displayed to the user.

```jsx
const [error, setError] = useState(null);  // Initially set to null, meaning no error
```

#### **Example of React State in Action:**

```jsx
const handleSearch = async (e) => {
  e.preventDefault();
  
  if (!searchQuery.trim()) return;  // Ignore empty queries
  if (loading) return;  // Prevent search if loading is true
  
  setLoading(true);  // Set loading to true while fetching the search results
  try {
    const searchResults = await searchMovies(searchQuery);  // Fetch search results
    setMovies(searchResults);  // Update movies state with search results
    setError(null);  // Reset any previous error
  } catch (err) {
    console.log(err);
    setError("Failed to search movies...");  // Set error message if fetch fails
  } finally {
    setLoading(false);  // Set loading to false once data is fetched
  }
};
```

In this example:
- **`setMovies`** is used to update the `movies` state with the fetched results.
- **`setLoading`** is used to toggle the loading state while fetching.
- **`setError`** is used to display an error message if something goes wrong.

### 2. **`useEffect` Hook**

The `useEffect` hook is used to perform side effects in function components. Side effects include things like data fetching, manually changing the DOM, or subscribing to external data.

#### **`useEffect` Basics**

The `useEffect` hook takes two parameters:
1. A function that contains the side-effect logic.
2. A dependency array that tells React when to re-run the effect (based on values changing).

```jsx
useEffect(() => {
  // Side effect logic goes here
}, [dependency]);  // The effect runs when "dependency" changes
```

#### **Examples of `useEffect` in the Movie App:**

- **Fetching Popular Movies on Component Mount**

In the `Home` component, the `useEffect` hook is used to fetch popular movies when the component mounts (i.e., when the page first loads).

```jsx
useEffect(() => {
  const loadPopoularMovies = async () => {
    try {
      const popularMovies = await getPopularMovies();  // Fetch popular movies
      setMovies(popularMovies);  // Update the movies state with the fetched data
    } catch (error) {
      console.log(error);
      setError("Failed to fetch data");  // Handle error if fetching fails
    } finally {
      setLoading(false);  // Set loading to false after data is fetched
    }
  };

  loadPopoularMovies();  // Call the function to load popular movies
}, []);  // Empty dependency array means this effect runs once when the component mounts
```

In this example:
- The **`useEffect`** hook runs only once when the component mounts, because the dependency array is empty (`[]`).
- It calls the `getPopularMovies` function to fetch the popular movies and update the state with the results.

- **Fetching Movies Based on Search Query**

Another `useEffect` can be used to trigger fetching data when the search query changes (i.e., when the user submits a new search).

```jsx
useEffect(() => {
  if (!searchQuery.trim()) return;  // Skip if the search query is empty
  
  const fetchMovies = async () => {
    try {
      setLoading(true);  // Set loading to true while fetching
      const results = await searchMovies(searchQuery);  // Fetch movies based on the query
      setMovies(results);  // Update the movies state with search results
    } catch (err) {
      console.error(err);
      setError("Failed to search movies");
    } finally {
      setLoading(false);  // Set loading to false after fetching
    }
  };

  fetchMovies();  // Trigger the fetch
}, [searchQuery]);  // The effect will run every time searchQuery changes
```

Here:
- **`searchQuery`** is added to the dependency array. Whenever `searchQuery` changes, the effect will run again to fetch new search results.

---

### 3. **LocalStorage and Context State**

The application uses **React Context** for managing the global state of favorite movies, which is persisted in `localStorage`. This ensures that even if the user refreshes the page, their favorite movies will still be saved.

#### **Movie Context with LocalStorage**

- **State Variables** in Context:
  - `favorites`: Holds the list of favorite movies.
  - `addToFavorites`: Adds a movie to the `favorites` list.
  - `removeFromFavorites`: Removes a movie from the `favorites` list.
  - `isFavorite`: Checks if a movie is already in the `favorites` list.

- **LocalStorage Integration**:
  - On initial load, the app checks `localStorage` for any saved favorites and updates the `favorites` state accordingly.
  - Whenever the `favorites` list changes, it is saved to `localStorage` to persist the data.

```jsx
useEffect(() => {
  const storedFavs = localStorage.getItem("favorites");  // Check localStorage for saved favorites
  if (storedFavs) setFavorites(JSON.parse(storedFavs));  // Set the state with stored favorites
}, []);

useEffect(() => {
  localStorage.setItem('favorites', JSON.stringify(favorites));  // Save favorites to localStorage whenever it changes
}, [favorites]);
```

This ensures that the favorite movies remain intact even after page reloads, making the app more user-friendly.

---

### 4. **API Variables**

- **API_KEY**: The API key is used to authenticate requests to The Movie Database (TMDb) API.
  
  Example(I did not protect my API key, but I will surely do so in production.):
  ```js
  const API_KEY = "8fae7448c58ef6fc6e99964176f5f206";  // Your API key
  ```

- **BASE_URL**: The base URL for the TMDb API, used to construct API requests.
  
  Example:
  ```js
  const BASE_URL = "https://api.themoviedb.org/3";  // Base URL for API
  ```

#### **API Functions**:

- **`getPopularMovies`**: Fetches a list of popular movies using the `fetch` API.
  ```js
  const response = await fetch(`${BASE_URL}/movie/popular?api_key=${API_KEY}`);
  const data = await response.json();
  return data.results;
  ```

- **`searchMovies`**: Fetches movies based on a search query.
  ```js
  const response = await fetch(
    `${BASE_URL}/search/movie?api_key=${API_KEY}&query=${encodeURIComponent(query)}`
  );
  const data = await response.json();
  return data.results;
  ```

---

## Conclusion

Understanding **React state** and **useEffect** is essential for building interactive applications. In this project, state is used to store dynamic data such as the search query, list of movies, and loading/error states. The `useEffect` hook helps in fetching data when the component mounts or when certain dependencies change (like the search query). Together, they allow the app to be responsive and provide a smooth user experience.

---



