## Configuration
```json
{
  "Max Routines (others will be stacked up in a queue)": 100,
  "Queue Check Time (Seconds)": 1.0,
  "Check Queue on Frame?": false,
  "Execution Warn Time": 150,
  "Print Stacktrace on Warn?": false,
  "Use scaled time for time calculations?": true
}
```

## Developers
*Note you have to include/require this plugin, hooks don't allow type parameters (however some methods allow hooks)*

### Implemented Methods/Hooks

Here are some methods I've already implemented

*Any timed reference are in seconds*
```csharp

/// <summary>
///     Start a <see cref="Coroutine" />
/// </summary>
/// <param name="coroutine">Target coroutine</param>
/// <returns><see langword="true" /> if the coroutine was stored & <see langword="false" /> if the coroutine is <see langword="null" /> or it was queued</returns>
/// This is a hook
bool StartCoroutine(Coroutine coroutine)

```

```csharp

/// <summary>
///     Create a <see cref="Coroutine"/>
/// </summary>
/// <param name="owner">Owner</param>
/// <param name="enumerator">The instructions (leave null to create instructions</param>
/// <param name="id">Id of this coroutine</param>
/// <param name="onComplete">Callback when the task completes</param>
/// <returns>A <see cref="Coroutine"/> to run</returns>
/// This is a hook
Coroutine CreateCoroutine(Plugin owner, IEnumerator instructions, string id = null, Action onComplete = null)

```

```csharp

/// <summary>
///     Stop a coroutine with a given id
/// </summary>
/// <param name="id">Given id</param>
/// <returns>If a coroutine was stopped</returns>
/// This is a hook
bool StopCoroutine(string id)

```

```csharp

/// <summary>
///     Stop coroutines started by <see cref="owner"/>
/// </summary>
/// <param name="owner">Target plugin</param>
/// This is a hook
void StopCoroutines(Plugin owner)

```

```csharp

/// <summary>
///     Start an asynchronous task that isn't repeated
/// </summary>
/// <param name="owner">Owner</param>
/// <param name="task">The task to complete</param>
/// <param name="initialDelay">Any initial delay</param>
/// <param name="id">Id for this coroutine</param>
/// <param name="onComplete">Callback for when the task is completed</param>
/// <returns>A <see cref="Coroutine"/> to run</returns>
/// This is a hook
Coroutine GetDelayedTask(Plugin owner, Action task, float initialDelay = 0f, Action onComplete = null)

```

```csharp

/// <summary>
///     The same as <see cref="GetDelayedTask" /> but it repeats
/// </summary>
/// <param name="owner">Owner</param>
/// <param name="continuePredicate">Predicate to keep repeating</param>
/// <param name="interval">Interval between each repetition</param>
/// <param name="initialDelay">Any initial delay</param>
/// <param name="id">Id of this coroutine</param>
/// <param name="onComplete">Callback when <see cref="continuePredicate" /> returns false (task is complete)</param>
/// <returns>A <see cref="Coroutine"/> to run</returns>
/// This is a hook
Coroutine GetAsynchronousRepeatingTask(Plugin owner, Func<bool> continuePredicate, float interval,
            float initialDelay = 0f, string id = null, Action onComplete = null)
           
```

```csharp

/// <summary>
///     Asynchronously loop through list (this loops through the whole list no stopping)
/// </summary>
/// <param name="owner">Owner</param>
/// <param name="callback">The callback for each item in the list</param>
/// <param name="list">Target list</param>
/// <param name="interval">Interval between each repetition</param>
/// <param name="startIndex">The index in the list to start at</param>
/// <param name="reverse">If it just should loop in reverse</param>
/// <param name="completePerTick">How many loops to complete each tick</param>
/// <param name="initialDelay">Any initial delay</param>
/// <param name="id">Id of this coroutine</param>
/// <param name="onComplete">Callback when list is done looping</param>
/// <typeparam name="T">Type parameter of <see cref="list" /></typeparam>
/// <returns>A <see cref="Coroutine" /> to run</returns>
Coroutine LoopListAsynchronously<T>(Plugin owner, Action<T> callback, IList<T> list,
            float interval, int startIndex = -1, bool reverse = false, int completePerTick = 1, float initialDelay = 0f,
            string id = null, Action onComplete = null)
            
```

```csharp

/// <summary>
///     Asynchronously loop through list
/// </summary>
/// <param name="owner">Owner</param>
/// <param name="callback">The callback for each item in the list (return false to stop looping)</param>
/// <param name="list">Target list</param>
/// <param name="interval">Interval between each repetition</param>
/// <param name="startIndex">The index in the list to start at</param>
/// <param name="reverse">If it just should loop in reverse</param>
/// <param name="completePerTick">How many loops to complete each tick</param>
/// <param name="initialDelay">Any initial delay</param>
/// <param name="id">Id of this coroutine</param>
/// <param name="onComplete">Callback when list is done looping</param>
/// <typeparam name="T">Type parameter of <see cref="list" /></typeparam>
/// <returns>A <see cref="Coroutine"/> to run</returns>
Coroutine LoopListAsynchronously<T>(Plugin owner, Func<T, bool> callback, IList<T> list,
            float interval, int startIndex = -1, bool reverse = false, int completePerTick = 1,
            float initialDelay = 0f, string id = null, Action onComplete = null)
            
```

```csharp

/// <summary>
///     Asynchronously search through a list (check if value is in list)
/// </summary>
/// <param name="owner">Owner</param>
/// <param name="target">Target value</param>
/// <param name="callback">Callback on complete</param>
/// <param name="list">Target list</param>
/// <param name="interval">Interval between loop</param>
/// <param name="startIndex">The index in the list to start at</param>
/// <param name="reverse">If it just should loop in reverse</param>
/// <param name="completePerTick">How many loops to complete each tick</param>
/// <param name="initialDelay">Any initial delay</param>
/// <param name="id">Id of this coroutine</param>
/// <returns>A <see cref="Coroutine"/> to run</returns>
/// <typeparam name="T">Type parameter of <see cref="list" /></typeparam>
Coroutine SearchListAsynchronously<T>(Plugin owner, T target, Action<bool> callback, IList<T> list,
            float interval, int startIndex = -1, bool reverse = false, int completePerTick = 1, float initialDelay = 0f,
            string id = null)
            
```

```csharp

/// <summary>
///     Asynchronously find a value from it's corresponding key in a <see cref="IDictionary{TKey,TValue}" />
/// </summary>
/// <param name="owner">Owner</param>
/// <param name="target">Target key (Key -> Value)</param>
/// <param name="callback">Callback with value</param>
/// <param name="dictionary">Target dictionary</param>
/// <param name="interval">Interval between loop</param>
/// <param name="completePerTick">How many loops to complete each tick</param>
/// <param name="initialDelay">Any initial delay</param>
/// <param name="id">Id of this coroutine</param>
/// <typeparam name="TK">Key type</typeparam>
/// <typeparam name="TV">Value type</typeparam>
/// <returns>A <see cref="Coroutine"/> to run</returns>
Coroutine SearchDictionaryAsynchronously<TK, TV>(Plugin owner, TK target, Action<TV> callback,
            IDictionary<TK, TV> dictionary, float interval, int completePerTick = 1, float initialDelay = 0f,
            string id = null)
            
```

```csharp

/// <summary>
///     Find a player from the server asynchronously
/// </summary>
/// <param name="owner">Owner plugin</param>
/// <param name="data">Data about the player (name/id)</param>
/// <param name="callback">Callback with player or <see langword="null" /></param>
/// <param name="includeName">Should we check for name?</param>
/// <param name="ignoreCase">Should we check name ignore case?</param>
/// <param name="interval">Interval between each repetition</param>
/// <param name="completePerTick">How many loops to complete each tick</param>
/// <param name="initialDelay">Any initial delay</param>
/// <param name="reverse">Start in reverse?</param>
/// <param name="startIndex">Start index</param>
/// <param name="id">Id of this coroutine</param>
/// <typeparam name="T"></typeparam>
void FindPlayerAsynchronously<T>(Plugin owner, string data, Action<T> callback, bool includeName = true,
            bool ignoreCase = false, float interval = 0.01f, int completePerTick = 15, float initialDelay = 0f,
            bool reverse = false, int startIndex = -1, string id = null)

```

### Errors/Warnings
*All stages/levels are zero-based*

Errors & warnings will provide you a current stage & level. The stage is the index of the current instructions (Enumerator). The level is the index of the recursive instructions (i.e if you return another Enumerator & an error arises, you will be at level 1).

### Searching
There is a method that you can implement to search through an item that isn't supported
```csharp
/// <summary>
///     Method to be implemented (searching)
/// </summary>
/// <param name="owner">Owner</param>
/// <param name="target">Target item</param>
/// <param name="item">Object to search in</param>
/// <param name="getSize">Get size method</param>
/// <param name="keepSearchGoing">Keep search going method</param>
/// <param name="handleSearchIndex">Handle search method</param>
/// <param name="callback">Callback with found object</param>
/// <param name="interval">Interval between each repetition</param>
/// <param name="completePerTick">How many loops to complete each tick</param>
/// <param name="initialDelay">Any initial delay</param>
/// <param name="id">Id of this coroutine</param>
/// <typeparam name="T">Type to search in</typeparam>
/// <typeparam name="TK">Type of <see cref="target" /></typeparam>
/// <typeparam name="TV">Type of <see cref="handleSearchIndex" /> return</typeparam>
/// <returns>A <see cref="Coroutine"/> to run</returns>
Coroutine SearchAsynchronously<T, TK, TV>(Plugin owner, TK target, T item, Func<T, int> getSize,
            Func<int, T, bool> keepSearchGoing, Func<int, TK, T, TV> handleSearchIndex, Action<TV> callback,
            float interval,
            int completePerTick = 1, float initialDelay = 0f, string id = null)
```

### Instructions
Each coroutine has an enumerator of instructions that are all derived from the `ICoroutineInstruction` class.

Here are a few already implemented instructions:
```csharp
WaitForSeconds(float time, bool realTime = true)
WaitForMilliseconds(float time, bool realTime = true)
WaitForBool(Func<bool> predicate)
```

If you are looking to create your own instruction, implement `ICoroutineInstruction` and edit `IsCompleted` to your liking.

If you are looking to use scaled time, refer to `OnTick(float deltaTime)`.

*You do not need to implement `Tick`to edit `IsCompleted`*

If you are confused about any of the implemenation, feel free to look at the current implementations I've added!

### Recursive Instructions
Inside a coroutine, you can yield return another set of instructions (`Enumerator` or another `Coroutine`) and the coroutine will complete those instructions before continuing

#### Recursive Instructions Example
*(Rust) This example loops through all the entities on the server every 5 seconds and prints their short prefab name*
```csharp
private void OnServerInitialized()
{
      var coroutine = Coroutines.Instance.GetCoroutine(this, InfiniteEntityLoop());
      Coroutines.Instance.StartCoroutine(coroutine);
}

private IEnumerator InfiniteEntityLoop()
{
    yield return new WaitForSeconds(5f);
    yield return Coroutines.Instance.LoopListAsynchronously(this, entity =>
    {
        PrintWarning(entity.ShortPrefabName);
    }, BaseNetworkable.serverEntities.entityList.Values.ToList(), 0.01f, completePerTick: 7);
    yield return InfiniteEntityLoop();
}
```

### Maximums
You can set a maximum amount of coroutines in the config, however, you can also set a maximum per id (i.e `test` -> 10). With this example, you can only have 10 coroutines with the id `test` ticking at once. Currently an "id" is cross-plugin, meaning if you register one, another plugin can override it or read it (might be changed). If a coroutine is attempting to run & the maximum has already been met, it is added to the queue which is check at an interval from the configuration.

To register a maximum simply call this hook:

*Note that this does not throw an `ArgumentException` if this id is already registered; it overwrites it*
```csharp
/// <summary>
///     Register a maximum amount of instances of an id
/// </summary>
/// <param name="id">Target id</param>
/// <param name="max">Target maximum</param>
/// This is a hook
void RegisterMax(string id, int max)
```

You can unregister it via:
```csharp
/// <summary>
///     Unregister a previously set maximum of instances from <see cref="RegisterMax"/>
/// </summary>
/// <param name="id">Target id</param>
/// <returns><see langword="true" /> if an item was removed from <see cref="_idToMax"/></returns>
/// This is a hook
bool UnregisterMax(string id)
```

## Examples

### Looping through entities (Rust)
```csharp
//Requires: Coroutines

private void PrintEntity(BaseNetworkable entity)
{
    PrintWarning(entity.ShortPrefabName);
}

private void OnServerInitialized()
{
    var entities = BaseNetworkable.serverEntities.entityList.Values.ToList();

    foreach (var entity in entities)
    {
        PrintEntity(entity);
    }
    //The method above finishes in ~10-12 seconds and drops the fps to 0 for the whole execution time (100% FPS loss)

    Coroutines.Instance.LoopListAsynchronously(this, new Action<BaseNetworkable>(PrintEntity), entities, 0.01f, -1, true, 7);
    //The method above finishes in ~15-20 seconds and doesn't affect performance

    //These calculations were done on a Ryzen 3700X with 3233 entities in the list
}
```

### Giving players random items (Rust)
```csharp
private const string Id = "RandomItems";
private const float Time = 60f;

private void Start()
{
    var coroutine = Coroutines.Instance.CreateCoroutine(this, Instructions(), Id);
    Coroutines.Instance.StartCoroutine(coroutine);
}

private IEnumerator Instructions() {
    yield return new WaitForSeconds(Time);
    yield return GiveItems();
    yield return Instructions();
}

private Coroutine GiveItems()
{
    return Coroutines.Instance.LoopListAsynchronously(this, player =>
    {
        if (player.IsDead() || player.IsWounded()) return;
        var itemDef = ItemManager.itemList[Random.Range(ItemManager.itemList.Count)];
        var itemAmount = itemDef.stackable > 1 ? Random.Range(itemDef.stackable) + 1 : 1;
        var item = ItemManager.Create(itemDef, itemAmount);
        if (itemDef.condition.enabled)
        {
            var max = itemDef.condition.max;
            item.condition = Random.Range(max/10, max);
        }
        player.GiveItem(item);
    }, BasePlayer.activePlayerList, 0.01f, completePerTick: 5, id: Id);
}

private bool Stop()
{
    return Coroutines.Instance.StopCoroutine(Id);
}
```

## FAQ

### Will this fix the lag on my server?
No, this plugin doesn't simply fix the lag on your server. This plugin is meant for developers, to improve the performance of high cost code.

### How do the timings/warnings work?
The execution time warnings are based off of the config's limit & also the execution time is added on to the owner plugin's hook time.

### Can I contribute/add to this?
Yes, feel free to share your ideas & thoughts on what you think will improve the plugin!
