---
layout: default
title: Disadvantage in Loop
---

# Disadvantage in Loop

Loop is a lua library help developer use oop method write lua scripts. It is easy to use and very powerful. However, 
there are some disadvantage when I using it. So I write this article to show these weak points and some solution.

## Problem of undefined/new member variables passed to default constructor

When you chose loop base/simple model writing the code, you can define your own constructor by 
implement `__init` function or just use the default one. The default constructor receives a table with 
the variable you define for it. If you read the source code, it eventually calls the function rawnew defined 
in loop/base.lua, which will set the "Class table" as metatable of the passed parameter(table). 
To simplify the case, I write two pieces of code below helping you understand the whole process:

create a vector3 instance using loop:

```lua
local oo = require("loop.base")
local vec3_t = oo.class({
    x = 1, 
    y = 2, 
    z = 3, 
    __tostring = function(self)
        return self.x .. "," .. self.y .. "," .. self.z
    end
})
-- NOTE: also valid in this way: { 
-- function vec3_t:__tostring()
--     return self.x .. "," .. self.y .. "," .. self.z
-- end
-- } NOTE end 

local v1 = vec3_t{ x = 10 }
print("v1 = " .. tostring(v1)) -- v1 = 10,2,3
```

create a vector3 instance by pure lua:

```lua
local vec3_t = { 
    x = 1, 
    y = 2, 
    z = 3, 
    __tostring = function(self) 
        return self.x .. "," .. self.y .. "," .. self.z
    end
}
vec3_t.__index = vec3_t -- important!!!
-- vec3_t= setmetatable ( vec3_t, {} ) -- not necessary

local v1 = { x = 10 }
local v1 = setmetatable ( v1, vec3_t )
print("v1 = " .. tostring(v1)) -- v1 = 10,2,3
```

In the code, I call the constructor but only pass one member variable -- x to it. So when using the instance 
of vector v1, it turns out those undefined member will go and get the value store in metatable, and return 
them when you call it, and that's great! So when you call v1.y, it will get 2. Even more, if you try to set 
the undefined value, for example, v1.y = 3, lua will be smart enough to put it in the source table(v1) that 
will prevent developer overwrite the value in metatable, in this time, y will be actually defined in v1.

By now everything looks fine until I got something to do with the metatable. So this time we still create `v1` 
as `vec3_t{x=10}`, but after that, I changes value defined in `vec3_t`, for instance: `vec3_t.z = 30`. 
The code looks like this:

```lua
local v1 = vec3_t{ x = 10 }
print("v1 = " .. tostring(v1)) -- v1 = 10,2,3
vec3_t.z = 30
print("v1 = " .. tostring(v1)) -- v1 = 10,2,30
```

So as you can see, in this time, the changes will affect v1, and that's not what we expect.

To solve the problem, we have two ways:

  - compare and copy the member in rawnew. (this is a good way plus it will improve the performance in searching member)
    * if member not defined, use metatable value but still defined it in source table. 
    * if the member is not in metatable, ignore it.
  - do not change metatable(Class) member once they defined. (I also like this way, since there isn't a reason to change Class define in runtime)

## You can't defined read-only member, you can't defined get/set method for a member property

Well, since the loop takes control of `__index` and `__newindex` method, you can't easily define read-only member for your class.
