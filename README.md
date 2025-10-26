# FAIR-Spotify  
### University of Twente | FAIR Data Engineering Project (2025–2026)

This repository contains the work of **Group 13** for the *FAIR Data Engineering* course at the University of Twente.  
The goal of the project was to transform a raw Spotify dataset into a **FAIR** (Findable, Accessible, Interoperable, Reusable) dataset using Semantic Web standards.

---

## Repository Structure

| Folder | File | Description |
|---------|------|-------------|
| **ontology/** | `ont.ttl` | Ontology created in Protégé describing music-related entities (tracks, artists, albums, playlists, genres, and audio features). |
| **data/** | `original.csv` | Original dataset exported from the Spotify API. |
|  | `data.ttl` | FAIRified dataset in RDF/Turtle format using schema.org and SKOS. |
| **metadata/** | `metadata.ttl` | Metadata describing the dataset using DCAT and Dublin Core Terms. |
| **.htaccess** |  | W3ID redirect rules linking the permanent identifiers to the files on GitHub. |
| **README.md** |  | This documentation file. |

---

## Persistent Identifiers (W3ID)

Each resource in this project has a **persistent and resolvable identifier** provided through [w3id.org](https://w3id.org/):

| Type | W3ID URI | Redirects To |
|------|-----------|--------------|
| Ontology | [https://w3id.org/FAIR-course-UT/2025-2026/group13/ont](https://w3id.org/FAIR-course-UT/2025-2026/group13/ont) | `ontology/ont.ttl` |
| Data | [https://w3id.org/FAIR-course-UT/2025-2026/group13/data](https://w3id.org/FAIR-course-UT/2025-2026/group13/data) | `data/data.ttl` |
| Metadata | [https://w3id.org/FAIR-course-UT/2025-2026/group13/metadata](https://w3id.org/FAIR-course-UT/2025-2026/group13/metadata) | `metadata/metadata.ttl` |

These identifiers remain valid even if the repository structure changes, ensuring long-term accessibility.

---

## Ontology

The ontology was developed in **Protégé** and provides the semantic structure of the dataset.  
We initially attempted to import the *Music Ontology (MO)*, but since the PURL was not accessible, we instead reused concepts from **schema.org** and **SKOS**.  

**Main classes:**
- `schema:MusicRecording` – Track  
- `schema:MusicAlbum` – Album  
- `schema:MusicPlaylist` – Playlist  
- `schema:Person` / `schema:MusicGroup` – Artist  
- `skos:Concept` – Genre  
- `ex:AudioFeature` – Optional class for Spotify audio features  

Custom properties under our namespace `ex:` were added for features such as tempo, energy, danceability, and loudness.  

**Ontology IRI:**
