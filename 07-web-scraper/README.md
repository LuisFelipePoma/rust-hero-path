# 07 - Web Scraper with Rate Limiting

## Description
Web scraper that fetches multiple URLs with concurrency control and rate limiting.

## Usage Example
```bash
$ scraper --urls "site1.com,site2.com,site3.com" --delay 1000 --max-concurrent 3
[site1.com] [OK] 200 OK - Content length: 15.2 KB - 3.2s
[site2.com] [OK] 200 OK - Content length: 8.4 KB - 2.1s
[site3.com] [FAIL] 404 Not Found - 0.5s

$ scraper --input urls.txt --output results/
$ scraper --list links.json --extract "title,description"
```

## Concepts to Learn
- HTTP client with `reqwest`
- Rate limiting with threads
- HTML parsing with `scraper` or `select`
- Concurrent crawling with limits
- Retry logic

## Semaphore Example
```rust
use tokio::sync::Semaphore;

struct Crawler {
    client: reqwest::Client,
    semaphore: Arc<Semaphore>,
}

impl Crawler {
    async fn fetch(&self, url: &str) -> Result<Html, CrawlError> {
        // Limit concurrent requests
        let _permit = self.semaphore.acquire().await?;
        
        let resp = self.client.get(url)
            .send()
            .await?
            .error_for_status()?;
        
        let html = resp.text().await?;
        Ok(Html::parse_document(&html))
    }
}
```

## Suggested Structure
```
web-scraper/
|-- src/
|   |-- main.rs
|   |-- crawler.rs      # Scraping logic
|   |-- parser.rs       # HTML parsing
|   |-- limiter.rs      # Rate limiting
|   `-- extractor.rs    # Data extraction
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- `reqwest` - HTTP client
- `tokio` - Async runtime
- `scraper` - HTML parsing
- `clap` - Argument parsing

## Resources
- [reqwest documentation](https://docs.rs/reqwest/)
- [tokio documentation](https://tokio.rs/tokio/tutorial)
- [scraper crate](https://docs.rs/scraper/)

## Difficulty Level
3/5 - Medium-Advanced, combines networking and parsing
