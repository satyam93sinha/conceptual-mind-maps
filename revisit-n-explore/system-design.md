# Cache
## Database Caching
- Tweaking DB cache settings for specific usage patterns can further boost performance. **HOW?**
- Caching at Object Level: Fetch from DB store as a class instance or object. Remove object from cache if underlying data has changed. **HOW?**
- Allows for asynchronous processing: workers assemble objects by consuming the latest cached object. **HOW?**
- When a new node is created due to failure or scaling, the new node will not cache entries until the entry is updated in the database. Cache-aside in conjunction with write through can mitigate this issue. **HOW?** Will all the entries from failed cache node re-update them into DB? 
- **Can you implement write-behind Cache update?**
- 