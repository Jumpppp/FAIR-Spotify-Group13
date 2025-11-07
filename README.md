# FAIR-Spotify

### University of Twente | FAIR Data Engineering Project (2025–2026)

This repository contains the work of **Group 13** for the *FAIR Data Engineering* course at the University of Twente.
The goal of this project was to transform a raw Spotify dataset into a **FAIR** (Findable, Accessible, Interoperable, Reusable) dataset using Semantic Web standards and best practices.

---

## Repository Structure

| Folder        | File              | Description                                                                                                                           |
| ------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **ontology/** | `ont.ttl`         | Ontology created in Protégé describing music-related entities such as tracks, artists, albums, playlists, genres, and audio features. |
| **data/**     | `original.csv`    | Original dataset exported from the Spotify API.                                                                                       |
|               | `data.ttl`        | FAIRified dataset in RDF/Turtle format following the ontology and schema.org vocabulary.                                              |
|               | `data-shapes.ttl` | SHACL constraints used to validate the FAIRified data.                                                                                |
| **metadata/** | `metadata.ttl`    | Metadata describing the dataset using DCAT and Dublin Core Terms.                                                                     |
| **.htaccess** |                   | W3ID redirect rules linking persistent identifiers to GitHub-hosted files.                                                            |
| **README.md** |                   | This documentation file.                                                                                                              |

---

## Persistent Identifiers (W3ID)

Each resource in this project has a **globally unique and resolvable identifier** provided through [w3id.org](https://w3id.org/).
This ensures long-term accessibility, even if the GitHub repository changes.

| Type     | W3ID URI                                                                                                                 | Redirects To                                                                                                         |
| -------- | ------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| Ontology | [https://w3id.org/FAIR-course-UT/2025-2026/group13/ont](https://w3id.org/FAIR-course-UT/2025-2026/group13/ont)           | [`ontology/ont.ttl`](https://raw.githubusercontent.com/Jumpppp/FAIR-Spotify-Group13/main/ontology/ont.ttl)           |
| Data     | [https://w3id.org/FAIR-course-UT/2025-2026/group13/data](https://w3id.org/FAIR-course-UT/2025-2026/group13/data)         | [`data/data.ttl`](https://raw.githubusercontent.com/Jumpppp/FAIR-Spotify-Group13/main/data/data.ttl)                 |
| Metadata | [https://w3id.org/FAIR-course-UT/2025-2026/group13/metadata](https://w3id.org/FAIR-course-UT/2025-2026/group13/metadata) | [`metadata/metadata.ttl`](https://raw.githubusercontent.com/Jumpppp/FAIR-Spotify-Group13/main/metadata/metadata.ttl) |

---

## Ontology

The ontology was developed using **Protégé** to define the semantic structure of our dataset.
Initially, the team attempted to reuse the **Music Ontology (MO)**, but since the PURL link was not resolvable, we replaced it with compatible terms from **schema.org** and **SKOS**.

**Main Classes**

* `schema:MusicRecording` → Track
* `schema:MusicAlbum` → Album
* `schema:MusicPlaylist` → Playlist
* `schema:Person` / `schema:MusicGroup` → Artist
* `skos:Concept` → Genre
* `ex:AudioFeature` → Represents Spotify’s numeric audio features (tempo, energy, danceability, etc.)

**Example Custom Properties (namespace `ex:`)**
`ex:tempo`, `ex:energy`, `ex:danceability`, `ex:loudness`, `ex:valence`, etc.
Each property has numeric ranges (0–1, 0–100, etc.) defined later in the SHACL constraints.

---

## Data Triplification

The raw CSV dataset was converted into **RDF triples** using the **RDF extension of OpenRefine**.
Each row (track) was represented as a `schema:MusicRecording` entity identified by a W3ID IRI.

**Mapping Example (CSV → RDF):**

| CSV Column        | RDF Predicate          | Example                                              |
| ----------------- | ---------------------- | ---------------------------------------------------- |
| `name`            | `schema:name`          | `"Die With A Smile"`                                 |
| `byArtist`        | `schema:byArtist`      | `"Lady Gaga"`                                        |
| `inAlbum`         | `schema:inAlbum`       | `"Die With A Smile"`                                 |
| `isPartOf`        | `schema:isPartOf`      | `"Today's Top Hits"`                                 |
| `datePublished`   | `schema:datePublished` | `"2024-08-16"^^xsd:date`                             |
| `genre`           | `schema:genre`         | `"pop"`                                              |
| `subGenre`        | `skos:broader`         | `"mainstream"`                                       |
| `energy`          | `ex:energy`            | `0.592`                                              |
| `danceability`    | `ex:danceability`      | `0.521`                                              |
| `dcterms:license` | —                      | `<https://opendatacommons.org/licenses/odc-by/1-0/>` |

Multi-artist tracks (e.g., “Lady Gaga, Bruno Mars”) were **split into separate triples** to maintain semantic consistency.

---

## Semantic Metadata Model

The metadata model enriches both the dataset and its variables:

* **Dataset-level metadata** uses **Dublin Core (dcterms)** and **DCAT** to describe title, creator, license, and distribution.
* **Variable-level metadata** describes each audio feature (e.g., energy, tempo, valence) using **RDFS** and **QUDT**:

  * `rdfs:label` → feature name
  * `rdf:type` → `qudt:QuantityKind`
  * `qudt:unit` → measurement unit (e.g., `unit:Percent`)
  * `minValue`, `maxValue` → numeric range
  * `rdfs:comment` → textual description

This ensures clear meaning and interoperability of every attribute in the dataset.

---

## SHACL Validation

The FAIRified RDF data was validated using **SHACL** constraints defined in `data-shapes.ttl`.
These shapes check that:

* All tracks have a `schema:name`, `schema:byArtist`, and `schema:url`.
* Numeric properties (like `ex:energy`, `ex:loudness`, etc.) fall within valid ranges (e.g., 0–1).
* `schema:datePublished` uses proper date formats.

Validation was tested in the SHACL Playground — no critical violations were found.

---

## Metadata and Data Publication

To make the resources accessible and persistent:

* All artefacts (data, ontology, metadata) are hosted on **GitHub**.
* **W3ID** redirects ensure permanent access via `https://w3id.org/FAIR-course-UT/2025-2026/group13/...`
* Each dataset entry (e.g., a track) can be directly resolved via its IRI.

Example triple:

```turtle
<https://w3id.org/FAIR-course-UT/2025-2026/group13/data#2plbrEY59IikOBgBGLjaoe>
  a schema:MusicRecording ;
  schema:name "Die With A Smile" ;
  schema:byArtist "Lady Gaga" ;
  schema:datePublished "2024-08-16"^^xsd:date ;
  dcterms:license <https://opendatacommons.org/licenses/odc-by/1-0/> .
```

---

## FAIR Improvements Achieved

| FAIR Principle    | Improvement                                                                                |
| ----------------- | ------------------------------------------------------------------------------------------ |
| **Findable**      | Assigned globally unique and persistent W3ID identifiers for ontology, data, and metadata. |
| **Accessible**    | All resources retrievable via HTTPS and publicly hosted on GitHub.                         |
| **Interoperable** | Used common vocabularies (schema.org, SKOS, DCTERMS, QUDT) for standardized descriptions.  |
| **Reusable**      | Included explicit license, provenance, and machine-readable metadata.                      |

---

## Contributors

**Group 13 — FAIR Data Engineering (2025–2026)**
University of Twente

| Name                     | Role                                               |
| ------------------------ | -------------------------------------------------- |
| Jump Srinualnad          | Data FAIRification, Ontology Design, Documentation |
| Anna Pantaloni           | Data FAIRification, Ontology Design, Documentation |                                                

---

This README can now serve as your **final project documentation** and **GitHub front page** — professional, complete, and aligned with the FAIR framework.
