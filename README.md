# useAsyncMemo
React hook for generating async memoized data.

## API

```typescript
function useAsyncMemo<T>(factory: () => Promise<T>, deps: DependencyList, initial?: T): T
```

If `factory` returns `undefined`, `useAsyncMemo` will leave the memoized value **unchanged**.

## Demo

A basic one:

```js
const [input, setInput] = useState()
const users = useAsyncMemo(async () => {
  if (input === '') return []
  return await apiService.searchUsers(input)
}, [input], [])
```

With ability of manual clearing:

```js
const [input, setInput] = useState()

const [clearFlag, setClearFlag] = useState({
  val: false
})
function clearItems() {
  setClearFlag({
    val: true
  })
}

const users = useAsyncMemo(async () => {
  if (clearFlag.val) {
    clearFlag.val = false
    return null
  }
  if (input === '') return []
  return await apiService.searchUsers(input)
}, [input, clearFlag], [])
```

With debounced value: (see [use-debounce](https://github.com/xnimorz/use-debounce))

```js
const [input, setInput] = useState()
const [debouncedInput] = useDebounce(input, 300)
const users = useAsyncMemo(async () => {
  if (debouncedInput === '') return []
  return await apiService.searchUsers(debouncedInput)
}, [debouncedInput], [])
```

