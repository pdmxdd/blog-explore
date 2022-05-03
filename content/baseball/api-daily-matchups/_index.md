---
title: "API: Daily Matchups"
weight: 105
date: 2022-05-03
---

Web scraping is almost always a less than ideal solution. When the web source changes you've got a pile of garbage.

A better solution is an API. Major League Baseball provides a stats API. It's predominately a paid resource, but they have a few endpoints you can access without a key.

## API Endpoint

I found the endpoint: `https://statsapi.mlb.com/api/v1/schedule?sportId=1&date=$2022-04-15`

Which results in a bunch of tasty JSON.

Example JSON:

```json
{
  "copyright" : "Copyright 2022 MLB Advanced Media, L.P.  Use of any content on this page acknowledges agreement to the terms posted here http://gdx.mlb.com/components/copyright.txt",
  "totalItems" : 15,
  "totalEvents" : 0,
  "totalGames" : 15,
  "totalGamesInProgress" : 0,
  "dates" : [ {
    "date" : "2022-04-15",
    "totalItems" : 15,
    "totalEvents" : 0,
    "totalGames" : 15,
    "totalGamesInProgress" : 0,
    "games" : [ {
      "gamePk" : 662485,
      "link" : "/api/v1.1/game/662485/feed/live",
      "gameType" : "R",
      "season" : "2022",
      "gameDate" : "2022-04-15T17:10:00Z",
      "officialDate" : "2022-04-15",
      "status" : {
        "abstractGameState" : "Final",
        "codedGameState" : "F",
        "detailedState" : "Final",
        "statusCode" : "F",
        "startTimeTBD" : false,
        "abstractGameCode" : "F"
      },
      "teams" : {
        "away" : {
          "leagueRecord" : {
            "wins" : 2,
            "losses" : 5,
            "pct" : ".286"
          },
          "score" : 3,
          "team" : {
            "id" : 109,
            "name" : "Arizona Diamondbacks",
            "link" : "/api/v1/teams/109"
          },
          "isWinner" : false,
          "splitSquad" : false,
          "seriesNumber" : 3
        },
...trimmed
}
```

### Copyright

One of the keys in the JSON response is the following [`copyright`](http://gdx.mlb.com/components/copyright.txt):

```txt
The accounts, descriptions, data and presentation in the referring page (the "Materials") are proprietary content of MLB Advanced Media, L.P ("MLBAM").  
Only individual, non-commercial, non-bulk use of the Materials is permitted and any other use of the Materials is prohibited without prior written authorization from MLBAM.  
Authorized users of the Materials are prohibited from using the Materials in any commercial manner other than as expressly authorized by MLBAM.
```

I can use this data. I'm not commercializing it, this blog has no adds, and does not make me any money.

I'm an individual, and I'm not bulk using the contents. I just make a simple request once a day.

## `jq`

Since I'm dealing with JSON I can use a JSON parsing tool.

Enter [`jq` a lightweight command-line JSON processor](https://stedolan.github.io/jq/).

Supposedly it's like `sed`I can slice, filter and map JSON structured data. That's exactly what I need.

### Transformations

```jq
'.dates[0].games[]'
jq '.status.detailedState, .gameDate[11:16], .teams.away.team.name, "(", .teams.away.leagueRecord.wins, "-", .teams.away.leagueRecord.losses, ")", "@", .teams.home.team.name, "(", .teams.home.leagueRecord.wins, "-", .teams.home.leagueRecord.losses, ")"'
```

`baseball.sh`:

```bash
# curl -s 'https://statsapi.mlb.com/api/v1/schedule?sportid=1&date=2022-04-09' > today.schedule

d=$(date +%Y-%m-%d)

curl -s "https://statsapi.mlb.com/api/v1/schedule?sportId=1&date=$d" > /home/pi/.baseball/today.schedule

cat /home/pi/.baseball/today.schedule | jq '.dates[0].games[]' | jq '.status.detailedState, .gameDate[11:16], .teams.away.team.name, "(", .teams.away.leagueRecord.wins, "-", .teams.away.leagueRecord.losses, ")", "@", .teams.home.team.name, "(", .teams.home.leagueRecord.wins, "-", .teams.home.leagueRecord.losses, ")"' > /home/pi/.baseball/daily.schedule.txt

touch /home/pi/.baseball/daily-schedule.csv
rm /home/pi/.baseball/daily-schedule.csv

echo "Today's ($d) Games:" > /home/pi/.baseball/daily-schedule.csv

sed 'N;N;N;N;N;N;N;N;N;N;N;N;N;N;s/\n/,/g' /home/pi/.baseball/daily.schedule.txt | sed 's/"//g' | sed 's/(,\([0-9]*\),-,\([0-9]*\),)/(\1-\2)/g' >> /home/pi/.baseball/daily-schedule.csv

rm /home/pi/.baseball/today.schedule
rm /home/pi/.baseball/daily.schedule.txt
```

#### Get the data

#### filter the parts we need

#### create csv

#### Add date to CSV

#### sed

`baseball`

```bash
#!/bin/bash

# curl -s https://www.baseball-reference.com/  | grep -zo "<h3>Today's Games[^,]\+,[^,]\+," | sed 's/(S)//g' | sed 's/<\/*[^>]*>//g' | sed '/^[[:space:]]*$/d' | awk '{$1=$1;print}' | sed -e '2,${N;N;N;s/\n/ /g}' | sed 's/^[A-Za-z]\+,//' | sed '/^[[:space:]]*$/d' > /home/pi/.baseball/bb-ref.schedule

bash /home/pi/.baseball/daily-schedule.sh

mail -s "MLB Games $(date)" 'paul@paulmatthews.dev' < /home/pi/.baseball/daily-schedule.csv
```

this is the cronjob

```cron
0 8 * * * bash /home/pi/.baseball/baseball
```

![daily email](pictures/baseball-email.png)