# Search Function Fix

## Issue

The search bug was in `playlist_logic.py` inside the `search_songs` function.

Before the fix, the function compared values backwards:

```python
if value and value in q:
```

That meant a search for `queen` would only match if the full artist value was inside the shorter query. For example, `queen` could match `queen`, but a normal partial search like `q` or a title search like `bohemian` would not work as expected.

The UI in `app.py` also limited each playlist search to the `artist` field:

```python
filtered = search_songs(songs, query, field="artist")
```

So users could not search for music by title, genre, or tag inside the Chill and Hype playlists.

## Change Made

`search_songs` now searches in the normal direction:

```python
if q in searchable_text:
```

It also searches across the song title, artist, genre, and tags by default. This lets each playlist tab search for music that belongs to that playlist.

The playlist UI in `app.py` now calls:

```python
filtered = search_songs(songs, query)
```

## Result

Searching inside the Hype playlist now filters only Hype songs.

Searching inside the Chill playlist now filters only Chill songs.

Both searches can match by:

- Title
- Artist
- Genre
- Tag
