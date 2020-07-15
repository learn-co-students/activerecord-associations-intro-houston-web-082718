```bash
[13] pry(main)> pop = Genre.create(name: "pop")
=> #<Genre:0x007fa34338d270 id: 1, name: "pop">
```

```bash
[14] pry(main)> pop.songs << rolling_in_the_deep
=> [#<Song:0x007fc75bb4d1e0 id: nil, name: "Rolling in the Deep", artist_id: nil, genre_id: nil>]
[15] pry(main)> pop.songs
=> [#<Song:0x007fc75bb4d1e0 id: nil, name: "Rolling in the Deep", artist_id: nil, genre_id: nil>]
[16] pry(main)> rolling_in_the_deep.genre
=> #<Genre:0x007fa34338d270 id: 1, name: "pop">
[17] pry(main)> pop.artists
=> [#<Artist:0x007fa342e34dc8 id: 1, name: "Adele">]
```

It's working!

## Video Reviews

* [ActiveRecord Associations](https://www.youtube.com/watch?v=5dqPYRsQd10) 

* [ActiveRecord Associations II](https://www.youtube.com/watch?v=l9JCzNN2Z2U) 

* [Aliasing ActiveRecord Associations](https://www.youtube.com/watch?v=WVBWlnUghOI)

* [Blog CLI with ActiveRecord and Associations](https://www.youtube.com/watch?v=ZfJ1rqFcNFU)

<p class='util--hide'>View <a href='https://learn.co/lessons/activerecord-associations-intro'>ActiveRecord Associations</a> on Learn.co and start learning to code for free.</p>
