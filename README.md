### Genres Have Many Songs and Have Many Artists

Create a file `app/models/genre.rb`. In it, define a class, `Genre`, to inherit from `ActiveRecord::Base`. 

```ruby
class Genre < ActiveRecord::Base

end
```

A genre can have many songs. Let's implement that with the `has_many` macro:

```ruby
class Genre < ActiveRecord::Base
  has_many :songs
end
```

A genre also has many artists through its songs. Let's implement this relationship with the `has_many through` macro:

```ruby
class Genre < ActiveRecord::Base
  has_many :songs
  has_many :artists, through: :songs
end
```

And that's it!

## Our Code in Action: Working with Associations

Go ahead and run the test suite and you'll see that we are passing all of our tests! Amazing! Our associations are all working, just because of our migrations and use of macros. 

Let's play around with our code. 

In your console, run `rake console`. Now we are in a Pry console that accesses our models. 

Let's make a few new songs:

```bash
[1]pry(main)> hello = Song.new(name: "Hello")
=> #<Song:0x007fc75a8de3d8 id: nil, name: "Hello", artist_id: nil, genre_id: nil>
[2]pry(main)> hotline_bling = Song.new(name: "Hotline Bling")
=> #<Song:0x007fc75b9f3a38 id: nil, name: "Hotline Bling", artist_id: nil, genre_id: nil>
```

Okay, here we have two songs. Let's make some artists to associate them to. In the *same PRY sessions as above*:

```bash
[3] pry(main)> adele = Artist.new(name: "Adele")
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">
[4] pry(main)> drake = Artist.new(name: "Drake")
=> #<Artist:0x007fc75b163c60 id: nil, name: "Drake">
```

So, we know that an individual song has an `artist_id` attribute. We *could* associate `hello` to `adele` by setting `hello.artist_id=` equal to the `id` of the `adele` object. BUT! Active Record makes it so easy for us. The macros we implemented in our classes allow us to associate a song object directly to an artist object:

```bash
[5] pry(main)> hello.artist = adele
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">
```

Now, we can ask `hello` who its artist is:

```bash
[6] pry(main)> hello.artist
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">
```

We can even chain methods to ask `hello` for the *name* of its artist:

```bash
[7] pry(main)> hello.artist.name
=> "Adele"
```

Wow!

Go ahead and do the same for `hotline_bling` and `drake`. 

We can also ask our artists what songs they have. Let's make a second song for adele first:

```bash
[8] pry(main)> someone_like_you = Song.new(name: "Someone Like You")
=> #<Song:0x007fc75b5cabc8 id: nil, name: "Someone Like You", artist_id: nil, genre_id: nil>
[8] pry(main)> someone_like_you.artist = adele
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">
```

Now let's ask `adele` for her songs:

```bash
[9] pry(main)> adele.songs
=> []
```

Huh? How can `adele`'s collection of songs be empty? We associated two songs with `adele`! Here's the thing, and this is important to remember:

**The model that `has_many` is considered the parent. The model that `belongs_to` is considered the child. If you tell the child that it belongs to the parent, *the parent won't know about that relationship*. If you tell the parent that a certain child object has been added to its collection, *both the parent and the child will know about the association*.**

Let's see this in action. Let's create another new song and add it to `adele`'s songs collection:

```bash
[10] pry(main)> rolling_in_the_deep = Song.new(name: "Rolling in the Deep")
=> #<Song:0x007fc75bb4d1e0 id: nil, name: "Rolling in the Deep", artist_id: nil, genre_id: nil>
```

```bash
[11] pry(main)> adele.songs << rolling_in_the_deep
=> [ #<Song:0x007fc75bb4d1e0 id: nil, name: "Rolling in the Deep", artist_id: nil, genre_id: nil>]
[12] pry(main)> rolling_in_the_deep.artist
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">
```

We added `rolling_in_the_deep` to `adele`'s collection of songs and we can see the `adele` knows it has that song in the collection *and* `rolling_in_the_deep` knows about its artist. 

Notice that `adele.songs` returns an array of songs. When a model `has_many` of something, it will store those objects in an array. To add to that collection, we use the shovel operator, `<<`, to operate on that collection, treat `adele.songs` like any other array. 

Let's play around with some genres and our has many through association. 

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
