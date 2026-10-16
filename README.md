# My React Snippets

Personal React snippets for JavaScript React (`.jsx`) and TypeScript React (`.tsx`) files.

## Try the snippets

1. Open this folder in VS Code.
2. Press **F5** to launch an Extension Development Host.
3. In a `.jsx` or `.tsx` file, type a prefix such as `comp`, `form`, or `state`, then select the snippet from IntelliSense.
4. Press **Tab** to move through placeholders.

Hook snippets insert the hook call only. Add the corresponding React hook import if it is not already in the file. The localStorage snippets use browser APIs and JSON serialization; `storage-state` is browser-only and should not be used during server-side rendering without a client-side guard.

## Prefixes

| Prefix | Snippet |
| --- | --- |
| `comp` | Default-exported function component |
| `comp-arrow` | Named arrow-function component |
| `comp-props` | Component using a prop |
| `form` | Controlled input form |
| `submit` | Form submit handler |
| `state` | `useState` declaration |
| `effect` | `useEffect` hook |
| `effect-cleanup` | `useEffect` hook with cleanup |
| `ref` | `useRef` declaration |
| `memo` | `useMemo` hook |
| `callback` | `useCallback` hook |
| `context` | `useContext` hook |
| `create-context` | Create a React context |
| `context-provider` | Context provider with shared state |
| `use-context` | Context access hook with provider check |
| `reducer` | `useReducer` hook |
| `reducer-fn` | Reducer function scaffold |
| `id` | `useId` hook |
| `custom-hook` | Custom hook scaffold |
| `async` | Async function with error handling |
| `async-handler` | Async event handler |
| `fetch-json` | Fetch JSON and check the response |
| `fetch-effect` | Fetch in an effect with abort cleanup |
| `storage-read` | Read and parse localStorage |
| `storage-write` | Serialize and write localStorage |
| `storage-effect` | Persist state to localStorage |
| `storage-state` | State initialized from and persisted to localStorage |
| `map` | Render an array with `map` |
| `li-key` | List item with a stable key |
| `map-li` | Array rendered as keyed list items |
| `button` | Button with type and click handler |
| `click` | Button click handler |
| `input` | Controlled text input |
| `textarea` | Controlled textarea |
| `checkbox` | Controlled checkbox |
| `select` | Controlled select |
| `loading` | Loading message or content |
| `error-state` | Accessible error message |
| `ternary` | Conditional two-branch JSX |
| `conditional` | Conditional JSX |
| `fragment` | React fragment |
| `class` | JSX `className` attribute |
| `image` | Image with alternative text |
