# Aggregation

[Lecture Slides: Aggregation.pdf](../Lecture-Slides/Lecture-11-Aggregation.pdf)

Class Example:

```sql
SELECT tracks.name as name, bytes
FROM tracks
JOIN albums on albums.AlbumId = tracks.AlbumId
JOIN artists on albums.ArtistId = artists.ArtistId
WHERE Bytes > 10000000 AND artists.Name LIKE "Z%";
```
