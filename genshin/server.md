# Setting up a custom server

WIP, look in the code of [mine] for now.

## Why?
**Can't you just make the request the server makes with Automate?**

Technically, yes, it is able to generate the required md5
checksums, but I found it would be easier to just let a
dedicated server handle that, since I already have a domain
and a vps.

## Automate
If you really want to keep it in Automate,
here's how you would generate the `DS` header:
```
Now ++ ",Noelle," ++ md5("salt=6s25p5ox5y14umn1p61aqyyvbvvl3lrt&t=" ++ Now ++"&r=Noelle", "h")
```
The salt should not be changed.
"Noelle" can be replaced by any other
6-character alphanuneric string.



[mine]: https://github.com/AroLeaf/resin-server
