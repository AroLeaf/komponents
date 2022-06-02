# Setting up a custom server

This is not a general guide on how to set up a web server.

## Requesting data from Hoyolab

You will need to send a GET request to:\
`https://bbs-api-os.hoyolab.com/game_record/genshin/api/dailyNote`\
with the following data:

### Query Parameters

- `role_id`: your UID
- `server`: the identifier of the server you play on.\
  One of these, based on the first character of your uid:
  - `6`: `os_usa`,
  - `7`: `os_euro`,
  - `8`: `os_asia`,
  - `9`: `os_cht`,


### Required Headers

#### Static

```
x-rpc-app_version: "1.5.0",
x-rpc-client_type: "5"
```

#### Dynamic

`Cookie:` Your Hoyolab cookies.
Check the [readme](readme.md#setup) on how to get them.

`DS:` A checksum header. It's structured as follows:
```
time = now()                                  // unix timestamp in seconds
salt = "6s25p5ox5y14umn1p61aqyyvbvvl3lrt"     // this exact salt is required
random = "Noelle"                             // or any other 6-character alphanumeric string

hash = md5("salt={salt}&t={time}&r={random}") // in hexadecimal
DS = "{time},{random},{hash}"
```

### Optional Headers

You may want to add these to make your request look more like one from the actual site.

```
Origin: "https://webstatic-sea.hoyolab.com",
Referer: "https://webstatic-sea.hoyolab.com",
Accept: "application/json, text/plain, */*",
Accept-Encoding: "gzip, deflate",
Accept-Language: "en-US;q=0.5",
x-rpc-language: "en-us",
User-Agent: "Mozilla/5.0 (X11; Linux x86_64; rv:100.0) Gecko/20100101 Firefox/100.0",
```

The `Accept-Encoding` can be set to `identity` if you want to receive uncompressed response data.

## Expected response data structure

- `again_after`: after how many seconds to request an update again
- `resin`: how much resin if left
- `resin_max`: how much resin you can have at most
- `bosses`: how many weekly bosses you can still get rewards from for 30 resin
- `bosses_max`: how many weekly bosses you can get rewards from for 30 resin per week
- `commissions`: how many daily commissions you have completed today
- `commissions_max`: how many daily commissions there are
- `commissions_rewarded`: if you collected your daily commission reward from Katheryne today
- `coins`: how much realm currency you have
- `coins_max`: how much realm currenncy you can have at most

They should be in this order! (blame Kustom for not having proper json decoding)

### Example response

```json
{
  "again_after": 480,
  "resin": 69,
  "resin_max": 160,
  "bosses": 2,
  "bosses_max": 3,
  "commissions": 3,
  "commissions_max": 4,
  "commissions_rewarded": false,
  "coins": 420,
  "coins_max": 2400,
}
```

### Example Mappings

Which fields from Hoyolab might correspond to fields sent by your server, where `notes` is `<hoyolab response data>.data`.

```js
{
  again_after           notes.current_resin < notes.max_resin 
                          ? parseInt(notes.resin_recovery_time) % 480 + 5
                          : 480
  resin:                notes.current_resin,
  resin_max:            notes.max_resin,
  bosses:               notes.remain_resin_discount_num,
  bosses_max:           notes.resin_discount_num_limit,
  commissions:          notes.finished_task_num,
  commissions_max:      notes.total_task_num,
  commissions_rewarded: notes.is_extra_task_reward_received,
  coins:                notes.current_home_coin,
  coins_max:            notes.max_home_coin,
}
```



[mine]: https://github.com/AroLeaf/resin-server
