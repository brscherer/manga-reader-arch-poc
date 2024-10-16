# Manga Reader - Frontend Architecture POC

Defining some Architectural concepts in a hypothetical system to explore architecture skills

## Folder Structure

- `public`: Public static files like the output files (html, images, etc)
- `src`: Main code base
  - `api`: API layer (Centralize connection with backend)
  - `assets`: Images, logos (before building for optimization)
  - `pages`: Entrypoints for each page (Home, Mangas, Profile)
  - `components`: All components, divided by features
    - `common`: Components not specific to features (usually atoms like Inputs, Button, Form, Menu, etc)
    - `manga`: Components related to manga (MangaCard, MangaList, etc)
    - `profile`: Components related to user profile (ProfileIcon, ProfileInfo, etc)
  - `hooks`: React hooks (useDebounce, useIntersectionObserver, etc)
  - `routing`: Specific to routing strategy (assuming we are not using file-system based routing like Next ou Gatsby)
  - `store`: State Management, like zustand or redux
  - `styles`: For global styles like reset.css
- `tests`: Unit and Integration tests

## Code Guidance

- Favor composability whenever you can
  - Build small and generic components instead of creating multiple components
  - If component is getting too many props to control inner state (when part of component is visible or not), favor `Compound` strategy
- Separate Container (how things work) from Presentational (how things looks like)
  - This is already started by separating `pages` from `components`, where `pages` should make the requests and `components` are only receiving data and displaying it. Its easy to get lost on this as the project grows so I explicitly call it out here
- ContextAPI is ok to avoid prop drilling, but favor State Management libraries to handle global state
  - ContextAPI use case will come handy if we have too much granularity in the component, and will serve to have a context for that specific feature
  - Redux/Zustand is more performatic for global states, like persisting user info throughout the app components
- API calls should all live under `api` folder, and error handling should be done there as well
- Use `<picture>` tags to handling images for different resolutions/network speed
  - As most of the app are images, we need to handle it in the best way possible to give a good experience for ours users
