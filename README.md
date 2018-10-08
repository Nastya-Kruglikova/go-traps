# go-traps

Concurrency 

1. Goroutines leak
```
func First(query string, replicas ...Search) Result {  
    c := make(chan Result)
    searchReplica := func(i int) { c <- replicas[i](query) }
    for i := range replicas {
        go searchReplica(i)
    }
    return <-c
}
```

2. Goroutines in loop
```
for _, val := range values {
	go func() {
		fmt.Println(val)
	}()
}
```

3. Mutex
```
func doSomething(){
    for {
        mu.Lock()
        defer mu.Unlock()
         
        // some interesting code
        // <-- the defer is not executed here as one *may* think
     }
   // <-- it is executed here when the function exits 
}
// Therefore the above code will Deadlock!
```

4. Mutex 2

```
func (ds *DataStore) get(key string) string {
 ds.Lock()
 defer ds.Unlock()
 if ds.count() > 0 { <-- count() also takes a lock!
  item := ds.cache[key]
  return item
 }
 return “”
}
func (ds *DataStore) count() int {
 ds.Lock() <-- deadlock
 defer ds.Unlock()
 return len(ds.cache)
}
```


https://play.golang.org/p/zNhoch6lbUE - math

https://play.golang.org/p/IhKZsllW9fl - maps

https://play.golang.org/p/2b_nT9vpKfN - Goroutines in main

https://play.golang.org/p/jRo-K6iRZy7 - copy

https://play.golang.org/p/YNDnetlocBa - update slice

https://play.golang.org/p/FEk_u6m4bWM - range channels

https://play.golang.org/p/UXKuyjMNeT4 - nil interfaces

https://play.golang.org/p/UkRbRpCSgfu- nil interfaces 2

https://play.golang.org/p/--dGYIcwpkI - json

https://play.golang.org/p/YBAtEgk7-IA - defer

https://play.golang.org/p/_WLUhppQcOn - defer 2

https://play.golang.org/p/_6rVx09cZAe - iota

https://play.golang.org/p/GHi-7u5X6FQ - select

https://play.golang.org/p/MWoO1EkT_ak - defer
