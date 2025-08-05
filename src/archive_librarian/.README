# Archive Librarian 
The Archive Librarian is a tool that aims to make dealing with historical job data from Condor easier. It ingests job record data from history files and writes it to a database - it can then answer queries and generate statistics for users. 
It is currently not a finished product. However, you can run a small demo by following the instructions under "How to run the Archive Librarian". 

## myHistoryList Overview
myHistoryList serves as  command-line tool for analyzing HTCondor job history data with flexible filtering and analysis options. Efficiently parses ClassAd files on-demand and provides various views of job execution statistics.

## Features
- Query jobs by user and cluster ID
- Multiple analysis modes (usage, files, batch, DAG, timing, location, status)
- Statistical summaries with min/max/mean/median calculations
- Efficient file reading with offset-based parsing
- Clean tabular output format

## How to run the Archive Librarian demo (myHistoryList)
```bash
# Build instructions here
make
# or
g++ -o myHistoryList [source files] [flags]
```

## Usage

### Basic Syntax
```bash
myHistoryList -user <username> -clusterId <clusterid> [analysis_flags]
```

### Required Arguments
- `-user <username>` - Filter by job owner
- `-clusterId <clusterid>` - Filter by cluster ID

### Analysis Flags (Optional)
- `-usage` - Show resource usage statistics (memory, wall clock time)
- `-files` - Show file paths (output, error, log, submit files)
- `-batch` - Show batch-related information
- `-dag` - Show DAG-related information  
- `-when` - Show timing information (job start dates)
- `-where` - Show execution location information
- `-status` - Show job status and exit codes

### Examples

#### Basic Query (Raw ClassAd Output)
```bash
myHistoryList -user aanand37 -clusterId 4327172
```

#### Single Analysis Mode
```bash
myHistoryList -user aanand37 -clusterId 4327172 -usage
```

#### Multiple Analysis Modes
```bash
myHistoryList -user aanand37 -clusterId 4327172 -usage -files -status
```

## Output Format

### Usage Analysis
- Table showing memory usage (MB) and wall clock time (seconds)
- Statistics: min, max, mean, median, total

### Files Analysis  
- Table showing output files, error files, user logs, and submit files

### Status Analysis
- Table showing job status, exit codes, and exit status
- Distribution statistics with counts and percentages

### Other Analysis Modes (-dag, -batch, -where, -when)
- Tabular format with relevant job information
- Summary statistics where applicable

## Performance
- Query execution times are logged to `librarian_query_times.txt`
- Efficient offset-based file reading minimizes I/O operations
- In-memory parsing with graceful error handling

## Architecture

### Key Components
- **Librarian**: Main query execution and file management
- **JobAnalysisUtils**: Parsing and output formatting utilities
- **Database Integration**: SQLite-based job record indexing

### Data Flow
1. Query database for job offsets
2. Read from archive files at specified offsets  
3. Parse ClassAd records into structured data
4. Apply analysis filters and generate output

## Main Files

### librarian.cpp
Core orchestration component that handles query execution, file management, and coordinates between file reading operations, database insertions, and querying interface functionality.

### readHistory.cpp
Handles the low-level reading and parsing of HTCondor history files.

### dbHandler.cpp
Manages SQLite database operations for job record indexing, including reading and writing.

### archiveMonitor.cpp
Monitors and manages HTCondor history file archives. Provides the main functionality for handling file rotation. 

### JobAnalysisUtils.cpp
Contains parsing utilities and output formatting functions for queries. 

### SavedQueries.h
Contains important SQL code:
    'SCHEMA_SQL'    - defines the schema of the database, including its tables, indexes, and constraints
    'GC_QUERY_SQL'  - defines the garbage collection policy for the database 
These queries are read in and saved upon initialization of the Librarian() class. 



## Current Status

**Note**: The system is currently hard-coded to read both epoch records and history records, but this should be made configurable in future versions.

**Main Function**: Currently serves as a demo of database capabilities. To run queries, you must:
1. Run the program
2. Navigate to the menu 
3. Select "1. Update" to populate the database
4. Select "2. Queries" to run actual queries

## Configuration
Config file accepts these attributes:
    epoch_history_path  - the path to the current epoch history file that's being written to 
    history_path        - the path to the current history file that's being written to
    db_path=            - the path to the database we'd like to write to (the librarian will build the database here, it shouldn't already exist)

The following attributes are not currently configurable, but could be made to be:
    jobCacheSize        - the maximum number of jobs that the incache map of processed jobs will hold
    dbSizeLimit         - the maximum size, in bytes, that the database will grow to 

## Dependencies
- C++20 or later
- SQLite3
- Standard library components (chrono, fstream, etc.)

## File Structure
```
├── librarian.cpp
├── readHistory.cpp  
├── dbHandler.cpp
├── archiveMonitor.cpp
├── JobAnalysisUtils.cpp
├── hist/
│   └── [test history files]
└── README.md
```

## Related Links
- Project Report: [Link to project report]
- CHTC Fellowship Website: [Link to CHTC fellowship website]

## Future Enhancements
- [ ] Configurable record type reading (epoch vs history)
- [ ] Direct query interface without menu navigation
- [ ] TTL-based caching implementation
- [ ] Additional analysis modes
- [ ] Export options (CSV, JSON)
- [ ] Web interface
- [ ] Batch processing capabilities