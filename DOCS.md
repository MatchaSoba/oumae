## App flow documentation

### App
This component handles when to display each child/section based on the fetch status. It also contains and handles logic for search input functionality.

### Search
The flow is as follows:
1. When user input is detected, `loading` state is set to `true`
2. Reads user input after a slight delay to allow user to finish typing
3. Updates `query` state after checking input is a valid query
4. Clears state on user command (Unimplemented)

### Autocomplete
This section is responsible for displaying suggested anime titles based on the user's input.
1. When the `query` signal is updated, `createEffect` watcher calls `fetchSuggestions` which queries the backend
2. Calls Jikan's API
   - If request is successful, updates `results`
   - If request timeout or is unsuccessful, sets `error` state to `true`
3. Sets `loading` to `false`, thus rendering suggestions

### Details
This component accepts a instance of the `Anime` class as a prop, using the info provided to fetch scores for the anime from a variety of sources.
1. When the `props.anime` signal is updated, `createEffect` watcher calls `fetchDetails` 
2. Calls APIs or scrapes websites, req/res follows the same flow as Autocomplete

### API Endpoints
Request: GET('/api/search')

Necessary url queries: 
- q: `string`
  
   The query, referring to the name of the media

- type: `"anime"` |  `"manga"` |  `"novel"`
  
   The type of media

Sucessful response: Media[] consisting of 5 elements



Request: POST('/api/details')

Request body: {
   media: Media
}

Sucessful response: DetailsApiResponse

### Development notes
These are things I feel the need to comment on during the development process:
- This is a frontend framework, backend http request libraries are either suboptimal (axios) or non-functional (phin), prefer native Fetch Api
- Solid's `useContext` and `createResource` features are wonky and produce unexpected results, such as losing all functionality after an error is encountered instead of recovering on the next change. I'll aim to phase these out and manually implement desired functionality.
- In order to use node.js web scraping libraries, a server backend is necessary. I will migrate all API queries and web scraping to the backend. The solid frontend will fetch all the necessary info from the backend instead of directly from external sites.