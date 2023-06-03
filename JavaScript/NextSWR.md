# SWR

````typescript
const { data, error, isLoading, isValidating, mutate } = useSWR(key, fetcher, options)
````

La fonction `fetcher` est une fonction asynchrone qui récupère la `key` (1er paramètre de useSWR) et renvoie les données en tant que `data`.

Exemple:

````typescript
import useSWR from "swr"

import fetcher from "@/libs/fetcher"

const useCurrentUser = () => {
    const { data, error, isLoading, mutate } = useSWR("/api/current", fetcher)

    return {
        data,
        error,
        isLoading,
        mutate
    }
}

export default useCurrentUser
````

````typescript
import axios from "axios"

const fetcher = (url: string) => axios.get(url).then((res) => res.data)

export default fetcher
````

````typescript
import useCurrentUser from '@/hooks/useCurrentUser'

const Sidebar = () => {
  const {data: currentUser} = useCurrentUser()
  .
  .
  .
````
