# Weeks 1-3 Restructure: Detailed Outline

## Overview of Restructuring

**Current State**:
- Week 1: Web Scraping mixed content
- Week 2: Data Validation + Labeling combined
- Week 4: HTTP/APIs separate

**New Structure**:
- **Week 1**: Data Collection (HTTP fundamentals → APIs → Web Scraping)
- **Week 2**: Data Validation (Pydantic → csvkit/jq → pandas analysis)
- **Week 3**: Data Labeling (Task types → Annotation tools → Quality metrics)

**Continuing Thread**: Netflix movie recommendation dataset
- Week 1: Collect the data
- Week 2: Validate and clean the data
- Week 3: Label additional features (sentiment, genres, etc.)

---

## Week 1: Data Collection for ML

### Lecture Outline (90 minutes, ~60-70 slides)

**Theme**: Building the Netflix movie dataset from scratch

#### Part 1: Understanding the Web (20 minutes)
**No emojis, clear slide breaks**

**Slide 1-2**: Title + Problem Introduction
- Title slide
- Netflix scenario: Need to predict movie success
- This is an ML problem, but first we need data

**Slide 3**: The ML Pipeline
- Full pipeline visualization (1 slide)
- Where we are: DATA COLLECTION

**Slide 4**: What Data Do We Need?
- List of features (split into categories)
- Basic info, performance metrics, production details
- One category per line, not crowded

**Slide 5**: The Challenge
- Data lives on different websites
- IMDb, Rotten Tomatoes, Box Office Mojo
- How do we get it?

**Slide 6**: Client-Server Model
- Simple diagram: Browser ↔ Server
- HTTP as the communication protocol

**Slide 7**: HTTP Request Anatomy
- URL structure breakdown
- Protocol, domain, path, query parameters
- Each part explained separately

**Slide 8**: HTTP Methods
- GET (retrieve data)
- POST (send data)
- One method per bullet, with example

**Slide 9**: HTTP Response
- Status codes table (split if needed)
- 200, 404, 500, 429
- What each means

**Slide 10**: Data Formats
- HTML vs JSON comparison
- When each is used
- Example of each (small snippets)

#### Part 2: Tool #1 - curl (15 minutes)

**Slide 11**: What is curl?
- Command-line HTTP client
- Why learn it: quick testing, universally available

**Slide 12**: Basic curl Usage
- Simple GET request
- Code example (not too long)

**Slide 13**: curl Options
- Save to file (-o)
- Follow redirects (-L)
- Show headers (-I)
- ONE option per slide if code is long

**Slide 14**: curl for APIs
- Example with OMDb API
- JSON response

**Slide 15**: Pretty Printing with jq
- What is jq
- Example: curl | jq
- ONE clear example

**Slide 16**: Why curl Matters
- Testing before coding
- Debugging APIs
- Works on servers/containers

#### Part 3: Tool #2 - Chrome DevTools (15 minutes)

**Slide 17**: Introduction to DevTools
- What it is
- How to access (F12, right-click inspect)

**Slide 18**: Network Tab
- Shows all HTTP requests
- Status codes, timing, headers

**Slide 19**: Network Tab Demo Setup
- Visit IMDb page
- Open Network tab
- Refresh to see requests

**Slide 20**: Inspecting Elements
- Elements tab
- Find data in HTML structure
- Right-click element → Inspect

**Slide 21**: Finding Data Sources
- Look for XHR/Fetch requests
- JSON responses
- API endpoints revealed

**Slide 22**: DevTools for API Discovery
- Many sites use background API calls
- DevTools reveals them
- Example: Twitter, Reddit

#### Part 4: Tool #3 - Python requests (20 minutes)

**Slide 23**: Why Python requests?
- Automation
- Can't use curl for 1000 movies
- Industry standard

**Slide 24**: Installing requests
- pip install requests
- That's it

**Slide 25**: First GET Request
- import requests
- response = requests.get(url)
- Print status code

**Slide 26**: Response Object
- status_code
- text
- content
- headers
- ONE attribute per line

**Slide 27**: Query Parameters
- Dict of parameters
- requests.get(url, params=...)
- Cleaner than string concatenation

**Slide 28**: Custom Headers
- Why: Some sites block Python's User-Agent
- headers dict
- Example with User-Agent

**Slide 29**: Error Handling - Network Errors
- Try-except for network issues
- Timeout parameter
- ConnectionError, Timeout exceptions

**Slide 30**: Error Handling - HTTP Errors
- Check status_code
- raise_for_status()
- Example code (short)

**Slide 31**: Complete Example
- Fetch one movie from OMDb
- With error handling
- Print key fields
- CODE SHOULD FIT ON ONE SLIDE

#### Part 5: Tool #4 - BeautifulSoup (15 minutes)

**Slide 32**: The HTML Problem
- We have HTML
- Need to extract specific data
- Example: rating buried in HTML

**Slide 33**: What is BeautifulSoup?
- HTML parser
- Makes HTML searchable
- pip install beautifulsoup4

**Slide 34**: Basic Usage
- from bs4 import BeautifulSoup
- soup = BeautifulSoup(html, 'html.parser')
- Now can search

**Slide 35**: Finding Elements - By Tag
- soup.find('h1')
- Gets first h1 element
- .text to get text content

**Slide 36**: Finding Elements - By Class
- soup.find('span', class_='rating')
- Note: class_ not class (Python keyword)

**Slide 37**: Finding Elements - By ID
- soup.find(id='main-content')
- Unique identifiers

**Slide 38**: find vs find_all
- find(): first match
- find_all(): all matches (returns list)
- When to use each

**Slide 39**: CSS Selectors
- soup.select('div.cast-list a')
- Copy from DevTools
- More powerful queries

**Slide 40**: Extracting Attributes
- Get href from link: link['href']
- Get src from image: img['src']
- Example

**Slide 41**: Complete Scraping Example
- Fetch IMDb page
- Parse with BeautifulSoup
- Extract title, rating, genre
- Return dict
- CODE FITS ON SLIDE

**Slide 42**: Scraping Challenges
- HTML structure changes
- Slow (sequential)
- Anti-scraping measures
- Legal/ethical concerns
- ONE issue per bullet

#### Part 6: Tool #5 - Playwright (10 minutes)

**Slide 43**: The JavaScript Problem
- Modern sites load data with JS
- requests + BeautifulSoup see empty page
- Example HTML before/after JS

**Slide 44**: What is Playwright?
- Headless browser automation
- Runs real browser
- Executes JavaScript

**Slide 45**: Installing Playwright
- pip install playwright
- playwright install chromium
- Downloads browser

**Slide 46**: Basic Playwright Usage
- Launch browser
- Navigate to page
- Wait for content
- Get HTML
- CODE EXAMPLE

**Slide 47**: Playwright Features
- Click buttons
- Fill forms
- Take screenshots
- Wait for elements
- ONE feature per bullet

**Slide 48**: When to Use Playwright
- JS-heavy sites (React, Vue, Angular)
- Need interaction (login, scroll)
- Infinite scroll
- When NOT to: slow, overkill for static sites

**Slide 49**: Performance Comparison
- requests: fast (< 1s)
- Playwright: slower (3-5s)
- Trade-offs

#### Part 7: APIs - The Better Way (20 minutes)

**Slide 50**: Problems with Scraping
- Fragile (HTML changes)
- Slow
- May be blocked
- Legal gray area

**Slide 51**: What is an API?
- Application Programming Interface
- URL that returns structured data
- JSON, not HTML

**Slide 52**: HTML vs JSON Comparison
- HTML example (messy)
- JSON example (clean)
- Same data, different format

**Slide 53**: REST APIs
- Representational State Transfer
- Common pattern
- GET /movies, GET /movies/123, etc.

**Slide 54**: OMDb API Introduction
- Open Movie Database
- Free API for movie data
- http://www.omdbapi.com/

**Slide 55**: Getting API Key
- Sign up process
- Free tier: 1000 requests/day
- Keep it secret

**Slide 56**: Using OMDb API
- requests.get with params
- apikey parameter
- response.json() to parse
- CODE EXAMPLE

**Slide 57**: JSON Response Structure
- Show actual OMDb response
- Title, Year, Rating, Genre, etc.
- Clean, structured

**Slide 58**: Search by IMDb ID
- More reliable than title search
- 'i' parameter instead of 't'
- Example

**Slide 59**: API Authentication
- API keys (in URL or header)
- Bearer tokens
- OAuth (mention only)

**Slide 60**: Rate Limiting
- APIs limit requests
- Example: 1000/day
- Status 429: Too Many Requests

**Slide 61**: Handling Rate Limits
- Respect limits
- Exponential backoff
- time.sleep() between requests
- CODE EXAMPLE (brief)

**Slide 62**: Pagination Concept
- APIs return data in pages
- page parameter
- Looping through pages

**Slide 63**: Pagination Example
- while loop
- Increment page number
- Break when no more data
- PSEUDOCODE, not full code

**Slide 64**: Best Practices
- Use environment variables for keys
- Handle errors
- Respect rate limits
- Cache responses
- ONE practice per bullet

**Slide 65**: APIs vs Scraping Summary Table
- Speed, Reliability, Format, Legal
- Clear comparison

**Slide 66**: Netflix Dataset - Complete Flow
- OMDb API for basic info
- (Optional) TMDb for more data
- Clean and merge
- Save to CSV
- Leads to Week 2

**Slide 67**: Summary - Tools Covered
- curl: Testing
- DevTools: Inspection
- requests: HTTP in Python
- BeautifulSoup: HTML parsing
- Playwright: JS sites
- APIs: Best option

**Slide 68**: Key Takeaways
- Data collection is first step
- APIs better than scraping when available
- Error handling is critical
- Real data is messy

**Slide 69**: Next Week Preview
- Week 2: Data Validation
- Pydantic, csvkit, pandas
- Clean the Netflix dataset

**Slide 70**: Resources
- API docs
- Tool documentation
- Practice APIs

---

### Lab Outline (3 hours, ~35-40 slides)

**Structure**: Progressive exercises, no emojis

#### Setup (10 min)
- Check Python version
- Install packages
- Get OMDb API key
- Verify installation

#### Part 1: Tool Mastery (60 min)

**Exercise 1.1**: curl Basics
- Test OMDb API from terminal
- View raw JSON
- Task: Get data for specific movie

**Exercise 1.2**: jq Formatting
- Install jq
- Pretty print JSON
- Extract specific fields
- Task: Get just title and rating

**Exercise 1.3**: Chrome DevTools
- Visit IMDb page
- Open Network tab
- Inspect elements
- Find rating in HTML

**Exercise 1.4**: DevTools API Discovery
- Find XHR/Fetch requests
- Look for JSON responses
- Questions to answer

#### Part 2: Python API Collection (60 min)

**Exercise 2.1**: First API Call
- Write get_movie() function
- Test with one movie
- Print results

**Exercise 2.2**: Environment Variables
- Create .env file
- Load with python-dotenv
- Move API key out of code

**Exercise 2.3**: Error Handling
- Add try-except
- Handle timeouts
- Handle invalid movies
- Test error cases

**Exercise 2.4**: Parse Response
- Extract useful fields
- Return clean dict
- Handle missing fields

**Exercise 2.5**: Batch Collection
- Loop over list of titles
- Collect multiple movies
- Progress indicator

**Exercise 2.6**: Save to CSV
- Use pandas
- Create DataFrame
- Export to CSV

**Exercise 2.7**: Challenge - Top 100
- Get IMDb Top 100 movie IDs
- Collect all 100
- Handle rate limits

#### Part 3: Web Scraping (Optional, 45 min)

**Exercise 3.1**: Fetch HTML
- Use requests to get IMDb page
- Add User-Agent header
- Save HTML to file

**Exercise 3.2**: Parse with BeautifulSoup
- Load HTML
- Find title element
- Find rating element
- Extract text

**Exercise 3.3**: Compare Approaches
- Time API call vs scraping
- Compare data quality
- Discuss trade-offs

**Exercise 3.4**: Playwright Demo
- Launch browser
- Navigate to page
- Wait for content
- Extract data

#### Part 4: Challenges (15 min)

**Challenge 1**: Multi-source collection
- Add TMDb API
- Merge data from both sources

**Challenge 2**: Async requests
- Use httpx
- Collect 100 movies in parallel

**Challenge 3**: Data validation
- Check for missing fields
- Validate data types

---

## Week 2: Data Validation & Quality

### Lecture Outline (90 minutes, ~60-70 slides)

**Theme**: Cleaning and validating the Netflix dataset from Week 1

#### Part 1: Why Data Validation? (10 minutes)

**Slide 1-2**: Title + Recap
- Week 1: Collected Netflix dataset
- Week 2: Is the data good?

**Slide 3**: The Messy Reality
- Real data has problems
- Missing values
- Wrong types
- Inconsistent formats

**Slide 4**: Types of Data Problems
- Missing values (NULL, N/A, empty)
- Type errors (string instead of number)
- Format inconsistencies (dates, currencies)
- Duplicates
- Outliers

**Slide 5**: Impact of Bad Data
- Garbage in, garbage out
- Models trained on bad data fail
- Need validation pipeline

#### Part 2: Command-Line Tools (25 minutes)

**Slide 6**: Why Command-Line?
- Fast exploration
- No coding needed
- Works on servers
- Pipeline friendly

**Slide 7**: Tool #1 - jq for JSON
- Query and transform JSON
- Filter, map, select

**Slide 8**: jq Basics
- Extract field: .Title
- Multiple fields: {title: .Title, year: .Year}
- CODE EXAMPLE

**Slide 9**: jq Filtering
- Select movies by year
- Filter by rating
- CODE EXAMPLE

**Slide 10**: jq Aggregation
- Count movies
- Average rating
- Group by genre
- ONE operation per slide if complex

**Slide 11**: Tool #2 - csvkit Introduction
- Suite of tools for CSV
- csvlook, csvstat, csvgrep, etc.

**Slide 12**: csvlook
- Pretty print CSV
- Example output

**Slide 13**: csvstat
- Statistics for each column
- Min, max, mean, null count
- Example output

**Slide 14**: csvgrep
- Filter rows
- Find specific movies
- CODE EXAMPLE

**Slide 15**: csvsql
- Query CSV with SQL
- Example query

**Slide 16**: csvjoin
- Join multiple CSV files
- Merge datasets
- Example

**Slide 17**: Command-Line Pipeline
- jq → csvkit → analysis
- Unix philosophy: small tools, pipes

#### Part 3: Pydantic for Validation (30 minutes)

**Slide 18**: What is Pydantic?
- Data validation library
- Type checking
- Automatic parsing

**Slide 19**: Why Pydantic?
- Catch errors early
- Document data structure
- Auto-generate docs
- Type safety

**Slide 20**: Basic Pydantic Model
- BaseModel class
- Field definitions with types
- CODE EXAMPLE (simple)

**Slide 21**: Movie Model Example
- Title: str
- Year: int
- Rating: float
- Genre: list[str]
- CODE (fits on slide)

**Slide 22**: Validation in Action
- Create instance with data
- Pydantic validates automatically
- Example with valid data

**Slide 23**: Validation Errors
- Wrong type
- Pydantic raises ValidationError
- Example with error

**Slide 24**: Optional Fields
- Use Optional[type]
- Handles missing data
- Example

**Slide 25**: Default Values
- Field with default
- Field(default=...)
- Example

**Slide 26**: Custom Validators
- @validator decorator
- Custom validation logic
- Example: year between 1900-2025

**Slide 27**: Field Constraints
- Field(gt=0, lt=10) for rating
- Field(min_length=1) for strings
- Example

**Slide 28**: Parsing JSON
- model.parse_obj(dict)
- model.parse_raw(json_string)
- Automatic validation

**Slide 29**: Exporting Data
- model.dict()
- model.json()
- Back to Python dict or JSON

**Slide 30**: Batch Validation
- List of models
- Validate 100 movies
- Collect errors
- CODE EXAMPLE

**Slide 31**: Error Reporting
- ValidationError details
- Which field failed
- Why it failed

#### Part 4: pandas for Analysis (25 minutes)

**Slide 32**: Loading the Dataset
- pd.read_csv()
- Load Netflix CSV from Week 1

**Slide 33**: Initial Inspection
- df.head()
- df.info()
- df.describe()
- ONE method per slide with output

**Slide 34**: Checking for Missing Values
- df.isnull().sum()
- Visualize missing data
- Which columns have nulls?

**Slide 35**: Handling Missing Values - Strategy
- Drop rows
- Fill with default
- Fill with mean/median
- When to use each

**Slide 36**: Dropping Missing
- df.dropna()
- df.dropna(subset=['rating'])
- Example

**Slide 37**: Filling Missing
- df.fillna(value)
- df.fillna(df.mean())
- Example

**Slide 38**: Type Checking
- df.dtypes
- Are ratings float?
- Are years int?

**Slide 39**: Type Conversion
- pd.to_numeric()
- pd.to_datetime()
- Handling errors='coerce'

**Slide 40**: Detecting Duplicates
- df.duplicated()
- Find duplicate movies
- Example

**Slide 41**: Removing Duplicates
- df.drop_duplicates()
- Keep first or last?
- Example

**Slide 42**: String Cleaning
- Strip whitespace
- Lowercase
- Remove special characters
- df['title'].str.strip()

**Slide 43**: Splitting Values
- Genre is comma-separated
- df['genre'].str.split(',')
- Example

**Slide 44**: Outlier Detection
- Box plots
- z-score
- IQR method
- ONE method per slide

**Slide 45**: Data Quality Report
- Percentage complete
- Percentage valid
- Number of outliers
- Summary statistics

#### Part 5: Complete Validation Pipeline (10 minutes)

**Slide 46**: Pipeline Overview
- Raw data → Validation → Cleaned data
- Steps visualization

**Slide 47**: Step 1 - Load and Inspect
- Read CSV
- Check shape, columns
- CODE

**Slide 48**: Step 2 - Pydantic Validation
- Validate each row
- Collect errors
- CODE

**Slide 49**: Step 3 - Clean
- Handle nulls
- Fix types
- Remove duplicates
- CODE

**Slide 50**: Step 4 - Transform
- Split genres
- Parse dates
- Normalize text
- CODE

**Slide 51**: Step 5 - Validate Again
- Check quality metrics
- Generate report

**Slide 52**: Step 6 - Export
- Save cleaned CSV
- Document changes

**Slide 53**: Automation
- Create validation script
- Run on any CSV
- Reusable

**Slide 54**: Netflix Dataset - Final
- Started with 200 movies
- Removed X invalid
- Fixed Y missing values
- Ready for ML

**Slide 55**: Summary - Tools Covered
- jq: JSON querying
- csvkit: CSV exploration
- Pydantic: Type validation
- pandas: Cleaning and analysis

**Slide 56**: Key Takeaways
- Validation catches errors early
- Multiple tools for different formats
- Automation is key
- Document your decisions

**Slide 57**: Next Week Preview
- Week 3: Data Labeling
- Manual annotation
- Quality metrics

**Slide 58**: Resources
- Pydantic docs
- pandas docs
- csvkit docs

---

### Lab Outline (3 hours, ~30-35 slides)

Similar progressive structure focusing on hands-on validation

---

## Week 3: Data Labeling & Annotation

### Lecture Outline (90 minutes, ~60-70 slides)

**Theme**: Adding human annotations to the Netflix dataset

#### Part 1: Introduction to Data Labeling (15 minutes)

**Slide 1-2**: Title + Recap
- Week 1: Collected data
- Week 2: Validated data
- Week 3: Add human labels

**Slide 3**: Why Labeling?
- Not all features can be automated
- Sentiment requires human judgment
- Quality ratings subjective
- Ground truth for training

**Slide 4**: Supervised Learning Reminder
- Model learns from labeled examples
- Input (features) → Output (label)
- Need lots of labeled data

**Slide 5**: Types of ML Tasks
- Classification
- Regression
- Segmentation
- Detection
- Generation
- Each needs different labels

#### Part 2: Labeling Tasks by Domain (25 minutes)

**Slide 6**: Vision Tasks Overview
- Image classification
- Object detection
- Segmentation
- Pose estimation

**Slide 7**: Image Classification
- Single label per image
- Example: Dog breed classification
- Tools: bounding box, labels

**Slide 8**: Multi-Label Classification
- Multiple labels per image
- Example: Movie genre from poster
- Can be Action AND Drama

**Slide 9**: Object Detection
- Bounding boxes + labels
- Example: Detect actors in frame
- Format: [x, y, width, height, class]

**Slide 10**: Segmentation
- Pixel-level labels
- Semantic vs Instance
- Example: Segment foreground/background

**Slide 11**: Text Tasks Overview
- Classification
- NER
- Sentiment
- Translation quality

**Slide 12**: Text Classification
- Assign category to text
- Example: Movie review → positive/negative
- Can be binary or multi-class

**Slide 13**: Named Entity Recognition (NER)
- Find and label entities
- Person, Organization, Location
- Example: Extract actor names from reviews

**Slide 14**: Sentiment Analysis
- Positive, Negative, Neutral
- Can be fine-grained (1-5 stars)
- Subjective task

**Slide 15**: Question Answering
- Given context, answer question
- Span selection
- Example: "Who directed Inception?"

**Slide 16**: Other Tasks
- Audio: Speech recognition, classification
- Video: Action recognition, tracking
- Time series: Anomaly detection

#### Part 3: Annotation Tools (20 minutes)

**Slide 17**: Why Specialized Tools?
- Manual labeling is tedious
- Need UI for efficiency
- Quality control features
- Export to standard formats

**Slide 18**: Tool #1 - Label Studio
- Open source
- Supports all task types
- Python-friendly
- We'll use this

**Slide 19**: Label Studio Features
- Web-based interface
- Customizable UI
- Multi-user support
- ML-assisted labeling

**Slide 20**: Installing Label Studio
- pip install label-studio
- label-studio start
- Open browser

**Slide 21**: Creating a Project
- Choose task type
- Upload data
- Configure labels

**Slide 22**: Labeling Interface
- Shows data item
- Annotation controls
- Submit and next

**Slide 23**: Export Formats
- JSON
- CSV
- COCO (for vision)
- Can convert to any format

**Slide 24**: Other Tools - Brief Mention
- Labelbox (commercial)
- CVAT (computer vision)
- Prodigy (NLP)
- roboflow (vision)

#### Part 4: Annotation Quality (20 minutes)

**Slide 25**: The Quality Problem
- Human annotators make mistakes
- Subjective tasks have disagreement
- How do we measure quality?

**Slide 26**: Inter-Annotator Agreement
- Multiple people label same data
- Compare their labels
- High agreement = clear task

**Slide 27**: Agreement Metrics
- Percentage agreement
- Cohen's Kappa (2 annotators)
- Fleiss' Kappa (3+ annotators)
- Krippendorff's Alpha

**Slide 28**: Percentage Agreement
- Simple: count matches / total
- Problem: doesn't account for chance
- Example calculation

**Slide 29**: Cohen's Kappa
- Agreement beyond chance
- Range: -1 to 1
- > 0.8 = strong agreement

**Slide 30**: Kappa Formula
- Show formula
- Explain terms
- Po = observed, Pe = expected

**Slide 31**: Kappa Example
- Two annotators, movie sentiments
- Show calculation
- Interpret result

**Slide 32**: Fleiss' Kappa
- Extension to 3+ annotators
- Same interpretation
- More robust

**Slide 33**: When Agreement is Low
- Task is unclear
- Guidelines need improvement
- More training needed
- May need expert annotators

**Slide 34**: Annotation Guidelines
- Critical for quality
- Define each label clearly
- Provide examples
- Handle edge cases

**Slide 35**: Guidelines Example
- Sentiment labeling rules
- What is positive?
- What is neutral?
- Ambiguous cases

**Slide 36**: Quality Control Strategies
- Gold standard examples (known labels)
- Regular audits
- Annotator training
- Consensus labeling

**Slide 37**: Gold Standard
- Expert-labeled examples
- Test annotators
- Calculate accuracy
- Filter bad annotators

**Slide 38**: Consensus Labeling
- Odd number of annotators (3, 5)
- Majority vote
- Discard ties or expert review

#### Part 5: Netflix Labeling Example (10 minutes)

**Slide 39**: Our Task
- Label movie plot sentiment
- Positive, Neutral, Negative
- Based on plot summary

**Slide 40**: Setup Label Studio
- Create sentiment project
- Upload 50 movie plots
- Configure 3 labels

**Slide 41**: Annotation Process
- Each person labels same 20 movies
- Export annotations
- Calculate agreement

**Slide 42**: Results Analysis
- Agreement percentage
- Cohen's Kappa
- Where did annotators disagree?

**Slide 43**: Improving Agreement
- Refine guidelines
- Discuss ambiguous cases
- Re-annotate

**Slide 44**: Final Dataset
- Week 1: Collected 200 movies
- Week 2: Cleaned to 180
- Week 3: Labeled 180 with sentiment
- Ready for ML!

**Slide 45**: Summary - Key Concepts
- Different tasks need different labels
- Tools make annotation efficient
- Quality metrics are essential
- Guidelines are critical

**Slide 46**: Best Practices
- Clear task definition
- Good guidelines with examples
- Multiple annotators
- Measure agreement
- Iterate on guidelines

**Slide 47**: Next Steps
- Weeks 4+: Use this data for ML
- LLM APIs, RAG, deployment
- The data work pays off

**Slide 48**: Resources
- Label Studio docs
- Annotation guideline templates
- Research papers on agreement metrics

---

### Lab Outline (3 hours, ~30-35 slides)

Hands-on annotation practice with Label Studio

---

## Implementation Notes

### Content Overflow Fixes

**Current issues**:
- Dense bullet lists (split into 2-3 slides)
- Long code examples (one concept per slide)
- Multiple concepts per slide (separate them)

**Rules for new slides**:
- Max 6-7 bullets per slide
- Code examples: max 15 lines
- One main concept per slide
- If explaining process, use multiple slides for steps

### Emoji Removal

Search and replace all:
- Remove all standalone emojis
- Replace "✅" with "Yes" or checkmark in text
- Replace "❌" with "No" or remove
- Replace "🎯" with "Goal:" or "Objective:"
- Replace "📊" with "Data:" or remove
- All others: remove or replace with text equivalent

### Slide Numbers

Already done in custom.css - all slides will show numbers

### Index Update

Will reference:
- week1-data-collection-lecture.md / week1-data-collection-lab.md
- week2-data-validation-lecture.md / week2-data-validation-lab.md
- week3-data-labeling-lecture.md / week3-data-labeling-lab.md

---

## Next Steps

1. Review this outline for approval
2. Implement Week 1 (lecture + lab)
3. Implement Week 2 (lecture + lab)
4. Implement Week 3 (lecture + lab)
5. Update index.qmd
6. Build all PDFs/HTMLs
7. Test that slide numbers appear
8. Verify no emojis remain
9. Check that no slides overflow

---

## Questions for Review

1. Is the slide count per section reasonable? (60-70 for lecture, 30-40 for lab)
2. Are the topic divisions clear?
3. Should any sections be expanded or condensed?
4. Is the Netflix thread consistent throughout?
5. Any tools or concepts missing?
