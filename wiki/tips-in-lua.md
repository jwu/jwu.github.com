---
layout: default
title: Tips In Lua
---

# Tips In Lua

## Swap variables

```lua
a, b = b, a
```

## If a is nil, copy b's value

```lua
a = a or b
```

**Note:** be-careful, if a is a boolean value and equals to false, then b always assigned.

```lua
a = false
a = a or b -- wrong: this will assign b as the value.
-- for good lua programming, always test nil.
```

## Short if-else a.k.a. ? operator

```lua
print(nil and 'true' or 'false') -- prints false
print(1 and 'true' or 'false') -- prints true
print(1>2 and 'true' or 'false') -- prints true
```

## Cache result by metatable

You can also use metatables to cache complex calculations.

```lua
cachedResults = {}
setmetatable(cachedResults, expensiveFunction)
expensiveFunction.__index = function(t,k)
    cachedResults.k = sqrt(sqrt(k^k))
    return cachedResults.k
end
```

Now calling 

```lua
print(cachedResults[50]) 
```
    
will be much faster after the first time. And sure you can do that in any language, but it's brief, elegant, and easy to understand in Lua. :-)


## Get/Set module in Global _G (from Programming in Lua 2ed Chapter 14)

A generalization of the previous problem is to allow fields in the dynamic name, such as “io.read” or “a.b.c.d”. If we write _G["io.read"], we do not get the read field from the io table. But we can write a function getfield such that getfield("io.read") returns the expected result. This function is mainly a loop, which starts at _G and evolves field by field:

```lua
function getfield (f) 
    local v = _G -- start with the table of globals 
    for w in string.gmatch(f, "[%w_]+") do
        v = v[w] 
    end
    return v 
end
```

We rely on gmatch, from the string library, to iterate over all words in f (where “word” is a sequence of one or more alphanumeric characters and underscores). The corresponding function to set fields is a little more complex. An assignment like a.b.c.d=v is equivalent to the following code: 

```lua
local temp = a.b.c
temp.d = v
```

That is, we must retrieve up to the last name and then handle it separately. The next setfield function does the task, and also creates intermediate tables in a path when they do not exist:

```lua
function setfield (f, v) 
    local t = _G              -- start with the table of globals 
    for w, d in string.gmatch(f, "([%w_]+)(.?)") do
        if d == "." then      -- not last field? 
            t[w] = t[w] or {} -- create table if absent
            t = t[w]          -- create table if absent
        else                  -- last field
            t[w] = v          -- do the assignment
        end 
    end
end
```

## Finite State Machine in lua

The Lua compiler optimizes tail calls into something resembling a goto. This allows for an interesting way to express a Finite State Machine where the individual states are written as functions and state transitions are performed by tail-calling the next state.

Coming from a background where tail calls are not optimized it looks a bit like recursion gone wild, of course.

```lua
-- Define State A
function A()
  dosomething"in state A"
  if somecondition() then return B() end
  if done() then return end
  return A()
end

-- Define State B
function B()
  dosomething"in state B"
  if othercondition() then return A() end
  return B()
end

-- Run the FSM, starting in State A 
A()
```

The example above is more than a little contrived, but it shows the key features. State transitions take the form "return NewState() end" and exiting the FSM completely is done by returning anything other than a tail call from any state.

Just make sure that you really are using tail calls, or you will get recursion gone wild after all... in particular

```lua
-- bad example!
function C()
  D()   -- note no return keyword here
end
```

does not tail call D. Instead, C explicitly returns nothing after calling D, which required a stack frame. In effect,

```lua
-- bad example!
function E()
  E()
end
```

will eventually overflow the stack, but

```lua
-- infinite loop hiding in the tail call
function F()
  return F()
end
```

will loop forever.

## A table merging and a table copying function

thanks to 

  * http://stackoverflow.com/questions/1283388/lua-merge-tables/1283608#1283608 
  * http://stackoverflow.com/questions/1397816/changing-one-table-seems-to-change-the-other/1400485#1400485

```lua
-- tableMerge:
-- merges two tables, with the data in table 2 overwriting the data in table 1
function tableMerge(t1, t2)
    for k,v in pairs(t2) do
        if type(v) == "table" then
            if type(t1[k]) ~= "table" then -- if t1[k] is not a table, make it one
                t1[k] = {}
            end
            tableMerge(t1[k], t2[k])
        else
            t1[k] = v
        end
    end
    return t1
end

-- tableCopy:
-- takes a table and returns a complete copy including subtables.
function tableCopy(t)
    return tableMerge({}, t)
end
```

## Lua C API: Delete metatable created with luaL_newmetatable?

How can I delete a metatable foo created with `luaL_newmetatable( L, "foo" );`, so that `luaL_getmetatable( L, "foo" );` will push a NIL value again?

Whatever the reason you may have to delete the metatable, it is possible. `luaL_newmetatable(L, "foo")` creates a table, which is stored in the Lua registry with the key "foo".

To delete the table, just set the field "foo" in the registry to nil. The code in C:

```c
lua_pushnil(L);
lua_setfield(L, LUA_REGISTRYINDEX, "foo");
```

Equivalent code in Lua:

```lua
debug.getregistry()["foo"] = nil
```

## Append Variable Parameters

source article: http://www.ac.net.blog.163.com/blog/static/13649056200932434539772/

```lua
function f(...)
    print("a", ...)
    print(..., "a")
end
```

When type:

```
f("1", "2")
```

the output is:

```
a 1 2
1 a
```

Cause variable parameters only affect when put it at the end of the function. There is two way to make it possible to put variable parameters in the front:

Method 1:

```lua
local tinsert = table.insert
local function append(x, ...)
    local t = {...}
    tinsert(t, x)
    return unpack(t)
end
```

Method 2:

```lua
local function append(x, arg1, ...)
    if arg1 == nil then
        return x
    else
        return arg1, append(x, ...)
    end
end
```

The Method 2 is faster when the variable is less than 10. Also it have an advantage that is it doesn't allocate on the heap so it will produce less times for invoke Garbage Collection.
