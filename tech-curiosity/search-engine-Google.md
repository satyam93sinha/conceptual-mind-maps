```markdown
# Search Engine Architecture (Google's Early Model)

## I. Introduction to Search Engines
* **Purpose:** Discover, organize, and retrieve information from the World Wide Web.
* **Core Challenge:** Dealing with the web's massive scale, diverse content, and dynamic nature.

## II. Web Crawling (Downloading Web Pages)
* **Goal:** Systematically download web pages from the internet.
* **Components & Process (Google Specifics):**
    * **Distributed Crawlers:** Several distributed programs responsible for fetching web pages.
    * **URLserver:**
        * Acts as a central dispatcher.
        * Sends lists of URLs to be fetched to the Distributed Crawlers.
    * **Storeserver:**
        * Receives fetched web pages from the Crawlers.
        * **Compresses** the web pages.
        * **Stores** them into the central **Repository**.
        * **Assigns `docID`:** Every new URL parsed from a web page receives a unique ID number called a `docID`.

* **Robots Exclusion Protocol (`robots.txt`):**
    * **Definition:** A standard text file (`robots.txt`) located at the website's root directory.
    * **Purpose:** Allows websites to communicate with web crawlers about which parts of the site should or should not be accessed/indexed.
    * **Directives:**
        * `User-agent`: Specifies which crawler the rules apply to (e.g., `*` for all, `Googlebot`).
        * `Disallow`: Instructs crawlers not to crawl specific paths (e.g., `/admin/`, `/private/`, `/`).
        * `Allow`: Overrides a broader `Disallow` rule for specific sub-paths (e.g., `/admin/public_docs/`).
        * `Sitemap`: Directs crawlers to the XML Sitemap for better discovery of pages.
    * **Key Characteristics/Limitations:**
        * **Honor System:** Relied upon by well-behaved crawlers; malicious bots may ignore it.
        * **Not a Security Measure:** Publicly viewable; doesn't prevent access to sensitive content.
        * **Doesn't Prevent Indexing Alone:** Pages may still be indexed if linked from other sites (need `noindex` meta tag for full exclusion).
        * **Helps Crawl Budget:** Guides crawlers to important content, avoiding resource waste on irrelevant sections.

## III. Data Storage (Repository & BigFiles)
* **Repository:**
    * The central storage for all downloaded and compressed web pages.
    * Each stored page is associated with its unique `docID`.
* **BigFiles (Underlying Storage System):**
    * **Concept:** Virtual files spanning multiple underlying file systems.
    * **Addressing:** Addressable by 64-bit integers (supports massive file sizes).
    * **Allocation:** Automatic allocation and deallocation across multiple file systems.
    * **File Descriptors:** Handles allocation/deallocation of file descriptors (overcomes OS limitations).
    * **Compression:** Supports rudimentary compression options.

## IV. Indexing
* **Goal:** Process raw web pages into efficient, searchable data structures.
* **Parsing:**
    * **Challenge:** Handling a "huge array of possible errors" in real-world HTML:
        * Typographical errors in tags.
        * Corrupted data (e.g., kilobytes of zeros).
        * Non-ASCII characters.
        * Excessively deep nesting of HTML tags (hundreds deep).
        * General malformed or non-standard HTML.
    * **Google's Solution:**
        * **Avoids YACC/CFG parsers:** Too strict and prone to failure on malformed input.
        * **Uses Flex (Lexical Analyzer):** Generates a fast scanner to break text into tokens.
        * **"Outfit with its own stack":** Custom parsing logic built on top of Flex; more robust and error-tolerant than a strict grammar parser.
        * **Result:** High speed and high robustness for parsing the entire web.

* **Lexicon (Vocabulary):**
    * **Definition:** The search engine's dictionary of all recognized words/terms.
    * **Key Optimization:** Designed to **fit entirely in main memory** (e.g., 256 MB for 14 million words).
    * **Implementation:**
        * **List of Words:** All words are concatenated together in a single memory block, separated by **null characters (`\0`)**.
        * **Hash Table of Pointers:** Used for fast lookup; stores pointers to the starting position of each word in the "List of Words".
    * **Content:** Contains 14 million words (rare words were excluded for space efficiency).
    * **Auxiliary Information:** Words may have additional metadata (details not fully explained in paper).

* **Hit Lists:**
    * **Definition:** A list of occurrences of a particular word within a specific document.
    * **Information per Hit:**
        * **Position:** Word's location in the document.
        * **Font:** Approximation of font size.
        * **Capitalization:** Whether the word was capitalized.
    * **Space Criticality:** Hit lists consume "most of the space" in both forward and inverted indices, hence efficient representation is vital.
    * **Encoding Chosen:** **Hand-optimized compact encoding (2 bytes per hit).**
        * **Plain Hits (Standard Text):**
            * 1 bit for capitalization.
            * 3 bits for **relative font size** (0-6; `111` is reserved as a fancy hit flag).
            * 12 bits for **word position** (0-4095; positions > 4095 are stored as 4096).
        * **Fancy Hits (Important Locations):**
            * Occur in URL, title, anchor text, or meta tags.
            * 1 bit for capitalization.
            * Font size field set to `111` (flag for fancy hit).
            * 4 bits to encode the **type of fancy hit**.
            * 8 bits for **position**.
            * **Anchor Hits (Special Fancy Hit):**
                * 8 bits of position split into:
                    * 4 bits for **position within anchor text**.
                    * 4 bits for a **hash of the `docID`** the anchor occurs in.
                * Enables limited phrase searching within anchors.
                * Anticipated future update for higher resolution.
        * **Relative Font Size Rationale:** Ensures documents aren't ranked higher simply for using a larger font; focuses on prominence within the document's structure.
    * **Hit List Length Storage:**
        * Stored immediately before the hits themselves.
        * **Combined with `wordID` (in forward index) and `docID` (in inverted index).**
        * Limits: 8 bits (for `wordID` context), 5 bits (for `docID` context).
        * **Escape Code:** If length exceeds bit limit, an escape code is used, and the actual length is stored in the next two bytes.

* **Forward Index (Document to Words):**
    * **Purpose:** Maps `docID`s to the words they contain and their hit lists.
    * **Barrels:**
        * Stored in **64 barrels** (partitions).
        * Each barrel covers a specific **range of `wordID`s**.
        * **Content:** If a document has words in a barrel's range, its `docID` is recorded, followed by the relevant `wordID`s and their `hitlists` for that document.
    * **Storage Trade-off:** `docID`s may be duplicated across multiple barrels if a document contains words spanning different `wordID` ranges (small storage overhead).
    * **Performance Benefit:** Saves significant time and complexity for the `Sorter` (the next phase), as the data is already partially sorted by `wordID` ranges.
    * **WordID Optimization:**
        * Instead of full `wordID`s, each `wordID` is stored as a **relative difference** from the minimum `wordID` of its respective barrel.
        * This allows `wordID`s to be stored in only **24 bits** (leaving 8 bits for hit list length within the combined entry).

* **URL to `docID` Conversion:**
    * **Checksums File:** A sorted file containing URL checksums mapped to their corresponding `docID`s. Sorted by checksum for efficiency.
    * **Single URL Conversion:** Compute URL checksum, then perform a **binary search** on the checksums file to find the `docID`.
    * **Batch Conversion (Crucial Optimization):**
        * Performed by the `URLresolver`.
        * Achieved by a **merge operation** with a list of URLs-to-convert (after their checksums are computed and sorted).
        * **Crucial Reason:** Avoids "one seek for every link" on disk (which would take >1 month for 322 million links). Batching allows sequential reads and fewer, more efficient disk I/O operations.

* **Links Database:**
    * Generated by the `URLresolver`.
    * Contains pairs of `docID`s representing all links (source `docID` -> target `docID`).
    * **Primary Use:** Used to compute **PageRanks** for all documents.

* **Sorter:**
    * **Input:** Takes the `Barrels` (which are partially sorted by `docID`).
    * **Task:** **Resorts** the data from the `Barrels` **by `wordID`** to create the **Inverted Index**.
    * **Efficiency:** Performed "in place" to minimize temporary disk space.
    * **Output:** Produces a list of `wordID`s and their offsets into the newly generated `Inverted Index`.

* **Inverted Index (Words to Documents):**
    * **Purpose:** The primary index for searching; maps words to the documents (and their `hitlists`) that contain them.
    * **Structure:** Sorted by `wordID`.

* **DumpLexicon:**
    * **Purpose:** A program that finalizes the lexicon for the searcher.
    * **Input:** Takes the list of `wordID`s and offsets (from the `Sorter`) and the initial lexicon (from the `Indexer`).
    * **Output:** Generates the `New Lexicon` (or `Lexicon_Searcher`), which is used by the search engine.

## V. Ranking (Scoring Documents for a Query)
* **Goal:** Combine various signals to determine the relevance of a document to a user's query.
* **Core Principle:** Designed so "no particular factor can have too much influence" (prevents manipulation and ensures balanced relevance).

* **Single-Word Query Ranking:**
    1.  **Hit List Examination:** Google looks at the document's `hit list` for the query word.
    2.  **Hit Type Classification:** Each hit is categorized into several `types` (e.g., `title`, `anchor`, `URL`, `plain text large font`, `plain text small font`).
    3.  **Type-Weights:** A vector of `type-weights` assigns importance to each hit type (e.g., title hits are more important).
    4.  **Count-Weights:** The raw number of hits for each type is converted into a `count-weight`:
        * Increases linearly at first.
        * **Quickly tapers off** (diminishing returns) to prevent keyword stuffing.
    5.  **IR Score Calculation:** Computed as the **dot product** of the `vector of count-weights` and the `vector of type-weights`.
    6.  **Final Rank:** The calculated `IR score` is combined with the document's **PageRank**.

* **Multi-Word Query Ranking:**
    1.  **Scanning Multiple Hit Lists:** Requires simultaneous scanning of hit lists for all query words.
    2.  **Proximity Importance:** Hits occurring close together in a document are weighted significantly higher.
    3.  **Matching Nearby Hits:** Hits from different query words are matched up if they appear close to each other.
    4.  **Proximity Computation:** A `proximity` value is calculated based on how far apart the matched hits are (within the document or anchor).
    5.  **Proximity Bins:** The continuous proximity value is classified into **10 different value "bins"**:
        * Ranging from a perfect "phrase match" (words side-by-side)
        * To "not even close" (words far apart).
    6.  **Combined Counts:** Counts are computed for **every type and proximity pair** (e.g., "title hit, phrase match"; "plain text, 10 words apart").
    7.  **Type-Prox-Weights:** Each unique type-proximity pair has its own `type-prox-weight`.
    8.  **IR Score Calculation:** Computed as the **dot product** of the `count-weights` (for each type-proximity pair) and the `type-prox-weights`.
    9.  **Final Rank:** The multi-word `IR score` is combined with **PageRank**.

* **Debugging & Development:**
    * A special debug mode allows displaying all ranking numbers and matrices with search results.
    * This capability was crucial for developing and refining the ranking system.

## VI. Query Processing & User Interface
* **Searcher:**
    * The core component that processes user queries.
    * Run by a **Web Server**.
    * **Utilizes:**
        * The `Lexicon` (built by `DumpLexicon`).
        * The `Inverted Index`.
        * The pre-computed `PageRanks`.
    * **Function:** To answer user queries by finding relevant documents and presenting them in ranked order.
* **Web Server:** Provides the user interface and interacts with the `Searcher` to display results.
```