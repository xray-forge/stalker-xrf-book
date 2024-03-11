# 🧺 Luabind

## todo;

todo; <br/>
todo; <br/>
todo; <br/>

## todo;

todo; <br/>
todo; <br/>
todo; <br/>

## todo;

todo; <br/>
todo; <br/>
todo; <br/>

## What is 'class' and 'super' in luabind

- [Luabind declaration of globals](https://github.com/OpenXRay/luabind-deboostified/blob/xray/src/open.cpp#L138)

It is 'luabind' part defined as globals

```c++
lua_setglobal(L, "class");

lua_pushcclosure(L, &make_property, 0);
lua_setglobal(L, "property");

lua_pushlightuserdata(L, &main_thread_tag);
lua_pushlightuserdata(L, L);
lua_rawset(L, LUA_REGISTRYINDEX);

lua_pushcclosure(L, &deprecated_super, 0);
lua_setglobal(L, "super");
```
