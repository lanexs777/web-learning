# React Query
React query is a remote state management library.

## Expected behavior
1. switching query keys will put query into hard query state again.
2. React Query doesn't know (or care) about status codes or network requests at all. It needs a resolved or rejected Promise that the queryFn needs to provide.

## Philosophy

Don't think of refetch for different id, think of I always want to see data for the current id. 

Think of qury key as useEffect's dependency, you input the right dependency, once dependency changed, react query will take care of the rest.

## Best Practice
1. InitialData should only be used if you have "real" data synchronously available - data that you'd happily cache for your users. The empty array is probably more of a "fallback" that you'd like to show until real data has been fetched. For that use-case, placeholderData is better

## FAQ

### 1. Update not shown
- Query ket not mathcing
```typescript
// these two are considered differenet
['item', '1']
['item', 1]
```
solution: use typescript and query key factories
- QueryClient is not stable
Query client holds cache, new client, new cache.

### 2. Different between `placeholderData` and `initialData`
[link](https://tkdodo.eu/blog/placeholder-and-initial-data-in-react-query)
