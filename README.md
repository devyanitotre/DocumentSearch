# Document Search Engine

A Java command-line application that searches a set of text documents for a given term and returns results ranked by relevance (match count). Implements three distinct search strategies.

---

## Project Structure

```
DocumentSearch/
├── documents/                        ← Text files to search
│   ├── warp_drive.txt
│   ├── hitchhikers.txt
│   └── french_armed_forces.txt
├── src/
│   ├── main/java/search/
│   │   ├── DocumentSearchApp.java    ← Main entry point
│   │   ├── Document.java             ← Loads a text file
│   │   ├── SearchResult.java         ← Holds filename + match count
│   │   ├── Searcher.java             ← Interface (contract) for all strategies
│   │   ├── StringSearcher.java       ← Method 1: plain string matching
│   │   ├── RegexSearcher.java        ← Method 2: regular expressions
│   │   └── IndexedSearcher.java      ← Method 3: pre-built word index
│   └── test/java/search/
│       └── SearcherTest.java         ← Unit tests
└── pom.xml                           ← Build configuration (Maven)
```

---

## Setup: Installing Java & Maven

### Step 1 — Install Java 17

**Mac:**
```bash
brew install openjdk@17
```
Then follow the printed instructions to add it to your PATH.

**Windows:**
Download the installer from https://adoptium.net and run it. Make sure to check "Set JAVA_HOME" during install.

**Verify it worked:**
```bash
java -version
# Should print: openjdk version "17.x.x" ...
```

---

### Step 2 — Install Maven (the build tool)

**Mac:**
```bash
brew install maven
```

**Windows:**
Download from https://maven.apache.org/download.cgi, unzip it, and add the `bin` folder to your PATH.

**Verify:**
```bash
mvn -version
# Should print: Apache Maven 3.x.x
```

---

## Running the Application

### 1. Navigate to the project folder
```bash
cd path/to/DocumentSearch
```

### 2. Build the project
```bash
mvn package -q
```
This compiles the code and produces a runnable JAR file at `target/document-search-1.0.jar`.

### 3. Run it
```bash
java -jar target/document-search-1.0.jar
```

### 4. Example session
```
========================================
        DOCUMENT SEARCH ENGINE          
========================================

Loaded 3 document(s):
  - french_armed_forces.txt
  - hitchhikers.txt
  - warp_drive.txt

Enter the search term (or 'quit' to exit): war
Search Method:
  1) String Match
  2) Regular Expression
  3) Indexed Search
Choose (1/2/3): 1

Search results (String Match) for: "war"
----------------------------------------
  french_armed_forces.txt             – 23 matches
  warp_drive.txt                      – 1 match
----------------------------------------
Elapsed time: 2 ms
```

---

## Running the Tests

```bash
mvn test
```

Expected output:
```
[INFO] Tests run: 10, Failures: 0, Errors: 0, Skipped: 0
[INFO] BUILD SUCCESS
```

---

## Adding Your Own Documents

Just drop any `.txt` file into the `documents/` folder and re-run the app. No code changes needed.

---

## The Three Search Methods Explained

| Method | How it works | Best for |
|---|---|---|
| **String Match** | Scans text with `indexOf` | Simple, no dependencies |
| **Regular Expression** | Uses Java `Pattern`/`Matcher` with word boundaries | Accurate whole-word matching |
| **Indexed Search** | Pre-builds a word→file→count map | Fastest for repeated searches |

---

## Notes for Production Readiness

If this were going to production, the next steps would be:

- **Persistent index** — save the index to disk (e.g. Lucene, Elasticsearch) so it survives restarts
- **Stemming** — treat "war", "wars", "warring" as the same root word
- **Concurrency** — process documents in parallel using Java's `ExecutorService`
- **REST API** — wrap the search in a Spring Boot endpoint instead of a CLI
- **Phrase search** — currently each word is indexed independently; phrase search requires position tracking
