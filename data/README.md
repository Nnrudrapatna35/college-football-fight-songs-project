# Fight Songs Data

### Description

`fight-songs.csv` contains data about fight songs from all schools in the Power Five conferences — the ACC, Big Ten, Big 12, Pac-12 and SEC — plus Notre Dame. Some schools have more than one fight song, and some of the songs sanctioned as “official” by their schools aren’t the ones that fans sing out. The songs that were chosen seemed best-known and best-loved. The lyrics used were those sung most regularly and also published by the school, so some verses were not included in the analysis. The tempo and length information come from the version of the song that is available on Spotify.

### Usage

`data(fight_songs)`

### Format

A data frame with 65 observations on colleges for the following variables.

* `school`: School name

* `conference`: School college football conference

* `song_name`: Song title

* `writers`: Song author

* `year`: Year the song was written; some values are `Unknown`

* `student_writer`: Was the author a student, yes or no? Some values are `Unknown`

* `official_song`: Is the song the official fight song according to the university, yes or no?

* `bpm`: Beats per minute

* `sec_duration`: Duration of the song in seconds

* `fight`: Does the song say "fight," yes or no?

* `number_fights`: Number of times the song says "fight"

* `victory_win_won`: Does the song say "victory," "win," or "won," yes or no?

* `rah`: Does the song say "rah," yes or no?

* `nonsense`: Does the song use nonsense syllables (e.g. "Whoo-Rah" or "Hooperay"), yes or no?

* `colors`: Does the song mention school colors, yes or no?

* `men`: Does the song refer to a group of men (e.g. men, boys, sons, etc.), yes or no?

* `opponents`: Does the song mention any opponents, yes or no?

* `spelling`: Does the song spell anything, yes or no?

* `trope_count`: Total number of tropes (`fight`, `victory_win_won`, `rah`, `nonsense`, `colors`, `men`, `opponents`, and `spelling`)

* `rank`: Team's AP College Football ranking

### References

Koeze, Ella, Neil Paine, and Sara Ziegler, "Our Guide to the Exuberant Nonsense of College Fight Songs". (2019) *fivethirtyeight*.
