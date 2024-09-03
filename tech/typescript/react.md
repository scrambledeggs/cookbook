# React Typescript Style Guide

## Custom Hooks

### Naming
:construction: TODO

### Basic Structure
- Create a type for parameters and return value of the custom hook

:x: DON'T use explicit return of type

```typescript
const useCustomHook = () => { //missing return type
  
  return value
}
```

:white_check_mark: DO explicit return of type
```typescript
type UseCustomReturn = {
  data: ...
}
const useCustomHook = (): UseCustomReturn => {
  return value
}
```
### Hooks that displays data


:construction: TODO


### Hooks that depend on a user action
The function of the hook must be called UPON a user action

**Note**: this pattern is based on apollo client `useMutation` hook

Common User Actions:
- Click
- Form Submission

```typescript
type CreateUserParams = {

}
type CreateUserResponse = {
  data: User
}
type User = {
  id: string
}


interface UseCreateUserStateBase {
  data?: User
  loading: boolean
  error?: Error
}

interface UseCreateUserStateSuccess extends UseCreateUserStateBase {
  data: User
  error?: undefined
}

interface UseCreateUserStateError extends UseCreateUserStateBase {
  data?: undefined
  error: Error
}
type UseCreateUserResponse = UseCreateUserStateSuccess | UseCreateUserStateError
type UseCreateUser = [
  (params: CreateUserParams) => Promise<UseCreateUserStateBase>,
  UseCreateUserStateBase
]
const postCreateUser = async (params: CreateUserParams): Promise<User> => { //notice the return of User type here
  //axios/fetch call here
  return user
}
const useCreateUser = (): UseCreateUser => {
  const [loading, setLoading] = useState(false)
  const [data, setData] = useState<undefined | User>()
  const [error, setError] = useState<Error | undefined>()
  const createUser = async (params: CreateUserParams): Promise<UseCreateUserResponse> => {
    // initital values
    setData(undefined)
    setError(undefined)
    setLoading(true)

    try {
      const user = await postCreateUser(params)
      setData(user)
      return { data: user, loading: false }
    } catch (e) {
      const error = e as Error
      setError(error)
      return { loading: false, error}
    } finally {
      setLoading(false)
    }
  }
  return [
    createUser,
    {
      data,
      loading,
      error
    }
  ]
}


// USAGE

const Component = () => {
  const [createUser, { data, loading, error}] = useCreateUser()
  const [name, setName] = useState('') // sample state for name text field
  const onSubmitForm = async () => {
    const { data, error } = await createUser({
      name
    })
    if (error) {
      // do something with the error
      alert('Error submitting form')
      return
    }
    // do something with the successful response
    appState.setUser(data)
    router.push('/next-page')
  }

  return // you can handle error/loading state with the form
}
```

### Tanstack Queries
:x: DON'T return `useQuery` directly
```typescript
const useGetUser = () => {
  return useQuery(...)
}
```

:white_check_mark: DO create a custom return type for the custom hook 

```typescript
type UseCustomHook = {
  data?: User
  loading: boolean,
  error?: Error
}
const useGetUser = (): UseCustomHook => {
  const { data, isLoading, error } = useQuery(...)
  return {
    data,
    loading: isLoading,
    error
  }
}
```
